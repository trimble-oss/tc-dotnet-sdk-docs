# Trimble.Connect.Data .NET SDK Developer Guide

### Content

1. [Local storage](#local-storage)
	+ [Creating the Local Storage](#creating-storage)
	+ [Accessing the Local Storage](#accessing-storage)
	+ [Thread safety and memory usage](#thread-safety)
2. [Local Object Storage](#object-storage)
3. [Local File Storage](#file-storage)
	+ [Accessing files in the local storage](#accessing-files)
	+ [Adding a files and folders to the local storage](#adding-files)
	+ [Modifying files in the local storage](#modifying-files)
4. [Sharing data](#sharing-data)
	+ [Local data and server data](#local-and-remote-data)
	+ [Synchronization scope](#sync-scope)
	+ [Synchronizing objects with remote storage](#objects-sync)
	+ [Using the Sync Client](#sync-client)
	+ [Pulling objects from a remote storage system](#pulling)
	+ [Pushing objects to a remote storage system](#pushing)
	+ [Conflict resolution](#conflicts)
	+ [Code snippets](#snippets)
		+ [Example: Pull cloud project to new local storage](#example-pull-project)
		+ [Example: Synchronize existing local storage with corresponding cloud project](#example-sync-existing)
		+ [Example: Create a cloud project from the local storage created offline](#example-push-project)
		+ [Example: How to create a local storage for the existing cloud project](#example-initial-pull)
	+ [Sharing files](#sharing-files)
		+ [Synchronizing files with a remote storage system](#sync-files)
		+ [Refreshing file entries from a remote storage system](#refresh-files)
		+ [Checking the file status](#file-status)
		+ [Resolving file conflicts](#file-conflicts)
		+ [Pulling files from a remote storage system](#pulling-files)
		+ [Pushing files to a remote storage system](#pushing-files)
		+ [Working with generated file formats](#generated-formats)
		+ [Example: Initial creation of the local storage for the cloud project](#example-formats)
	+ [Commands](#commands)
		+ [Using command queues](#using-commands)

### Acronyms

TID - Trimble Identity

TC - Trimble Connect

## <a name="local-storage">Local Storage</a>

The **Local Storage** component provides a solution for storing TC data locally offline on any device. The stored data can be easily shared between multiple devices via TC backend.

The local storage is designed for storing TC-specific data with explicit data schema supported by TC. The objects are stored independently from each other and cannot contain any logic or constraints.  Referential integrity between objects is not enforced or maintained.

Each storage instance is designed to store data from a single TC project and owned by a single user.

Local storage has the capability to store both [objects](#object-storage) and [files](#file-storage). 

The folder used in the storage constructor must initially. The Local Storage component will initialize it. 

A small metadata file ".storage" is created in the file storage directory. This file contains the information necessary for tracking the files and synchronizing them with a remote data storage system.

All objects are stored locally in a single file.  Local folders may be used as local "working folders" as well.

### <a name="creating-storage">Creating Local Storage</a>

Use the Create method of the _Storage_ class to create a new storage object bound to the project and user.  After storage creation, user and project entities cannot be changed.

    using (var storage = Storage.Create("directory path"), user, project)
    {
        // use the storage here
    }

### <a name="accessing-storage">Accessing Local Storage</a>

Use the Storage class to access the local storage objects and data.

    using (var storage = new Storage("directory path"))
    {
        // use the storage here
    }

The recommended location for the data file is the local (non-roaming) application data folder or the user's personal document folder.

### <a name="thread-safety">Thread safety and memory usage</a>

The _LocalStorage_ instances are not thread safe.  However, you can create multiple instances that point to the same data file and use them from different threads in a safe manner.  For convenience, _Storage_ class has an overloaded constructor that will allow you to create new connection to the same data file.

    using (var storageConn2 = new Storage(storage))
    {
        // use the storageConn2 here
    }

Each Storage instance maintains a small (a few megabytes) cache in memory.  It's recommended to release your in-memory _Storage_ instances by calling Dispose() method to free the cache and to reduce the application memory footprint.

## <a name="object-storage">Local Object Storage</a>

TC entity types are available through their corresponding collections: Persons, Todos, Comments, Snapshots, etc.

<!--### Accessing the stored objects

TODO

### Saving objects to the local storage

TODO

### Deleting objects

TODO

### Extensibility-->

Local storage provides a way for extending explicit data schema by adding attached property columns.  They can be identified by their concatenated string of ID and property name.  Setting user ID as target ID makes the property user-specific.

**Example:**

    private string AttachedPropertyKeyHeadForUser
    {
        get { return string.Format("AttachedPropertyName.{0}.", this.userId); }
    }

    private string AttachedPropertyKey(string localObjectIdentifier)
    {
        get { return string.Format("AttachedPropertyName.{0}.{1}", this.userId, localObjectIdentifier); }
    }

    public IEnumerable<LocalObject> GetAttachedPropertiesForUser()
    {
        if (this.userId == null)
        {
            return new List<LocalObject>();
        }
        
        var allProjectProperties = this.storage.GetProperties(this.project);
        
        return allProjectProperties
            .Where(pair => pair.Key.StartsWith(this.AttachedPropertyKeyHeadForUser))
            .Select(pair => JsonConvert.DeserializeObject<LocalObject>(pair.Value));
    }

    public void Save(LocalObject attachedObject)
    {
        if (this.userId == null)
        {
            return;
        }
        
        this.storage.SetProperty(this.project, 
    
        this.AttachedPropertyKey(attachedObject.Identifier), 
        
        JsonConvert.SerializeObject(attachedObject)); 
    }

    public void Delete(string localObjectIdentifier)
    {
        if (this.userId == null)
        {
            return;
        }
 
        this.storage.RemoveProperty(this.project,     

        this.AttachedPropertyKey(localObjectIdentifier));
    }

    public class LocalObject : NamedEntity    {  	}

## <a name="file-storage">Local File Storage</a>

In addition to objects, the local data storage component provides a way to store files locally on any device. The stored files can be easily shared between multiple devices via a TC backend.

### <a name="accesing-files">Accessing files in local storage</a>

The files are available through the _Files_ collection.

    foreach (var item in storage.Files)
    {
        Console.WriteLine("identifier: {0}", item.Identifier);
        Console.WriteLine("type: {0}", item.Type);
        Console.WriteLine("name: {0}", item.Name);
        Console.WriteLine("path: {0}", item.GetPath());
        Console.WriteLine("size: {0}", item.Size);
    }

The Files collection gives a plain list of all files and folders on the local system.  Note that there are both files (NodeType.File) and folders (NodeType.Folder).

It is also possible to iterate the file hierarchy using the parent-child relationship between folders and files.  The root folder can be found using the _RootFolder_ property in the Files collection.

    foreach (var item in storage.Files.RootFolder.Children)
    {
        Console.WriteLine("identifier: {0}", item.Identifier);
        Console.WriteLine("type: {0}", item.Type);
        Console.WriteLine("name: {0}", item.Name);
        Console.WriteLine("path: {0}", item.GetPath());
        Console.WriteLine("size: {0}", item.Size);
    }

The Files colleciton supports several query methods _WithXXX()_ for filtering the collection.  The example below lists all files in the folder with identifier "xxx".

    foreach (var item in storage.Files.WithType(NodeType.File).WithParentIdentifier("xxx"))
    {
        Console.WriteLine("identifier: {0}", item.Identifier);
        Console.WriteLine("type: {0}", item.Type);
        Console.WriteLine("name: {0}", item.Name);
        Console.WriteLine("path: {0}", item.GetPath());
        Console.WriteLine("size: {0}", item.Size);
    }

### <a name="adding-files">Adding a files and folders to local storage</a>

A folder is added with the _AddFolder()_ method exposed by the _LocalFile_ object that represents a folder the new folder is added to.  The method takes the new folder name as a parameter.

    var folder = storage.Files.RootFolder;
    var file = folder.AddFolder("Model");

A file is added with the _AddFileAsync()_ method exposed by the _LocalFile_ object that represents a folder the new file is added to.  The method takes the full file name as a parameter.

    var folder = storage.Files.RootFolder;
    var file = await folder.AddFileAsync("c:/Model/large_model.zip");

At this point, the file content is copied to the internal storage and a file entry has been added to the Files collection.  The _AddFileAsync()_ method returns the location of new entry.  Once the entry has been created, the file content is accessible from local storage using the _GetPath()_ method.  Notice that the file path in local storage does not contain the original file name, but uses a file content hash as a filename concatenated the original extension.

### <a name="modifying-files">Modifying files in local storage</a>

The files in local storage are marked as read only and should not be modified directly.  If a local disk item (file or folder) has not been published to the TC backend it can be removed from the repository using _Delete()_ method.

    // delete all files in the local storage
    foreach (var file in storage.Files)
    {
       var path = file.GetPath();
       file.Delete();
       if (File.Exists(path))
       {
          File.SetAttributes(path, FileAttributes.Normal);
          File.Delete(path);
       }
    }

To update a file with new content, the _UpdateFileAsync()_ method should be used.  Your app should not directly modify files in local storage.

## <a name="sharing-data">Sharing data</a>

In addition to storing data locally, the local storage component provides an easy way to share data between multiple devices using TC backend services.

### <a name="local-and-remote-data">Local data and server data</a>

Local storage uses an _edit-and-merge_ model where the shared data is downloaded into a private working copy.  The data can then be modified without any connection to the outside world.  In case that data has been modified both on the server and locally, the SDK will default to using the the action with the latest timestamp as the source of truth.  This is explained in more detail later in this documentation.

### <a name="sync-scope">Synchronization scope</a>

Based on the dependencies between entities and supported user scenarios, the following synchronization scopes were implemented: 

1. Project descriptor, last visited timestamp

2. Project members and user groups

3. Files, folders, and model alignments

    1. Content for files can be selectively synchronized

4. File comments

5. ToDos 

6. ToDo comments

7. Views 

8. View groups

### <a name="objects-sync">Synchronizing objects with remote storage</a>

There are two operations that combine together to synchronize local data with remote storage:
     1.  Push (from local to remote)
     2.  Pull (from remote to local)

Both _push_ and _pull_ stop when an error occurs (an exception will be propagated to the caller, changes up to that moment will be saved).

In the push operation, all objects which have been in any way modified locally are pushed to the server.  This includes newly created objects, modified objects, and deleted objects.  One possible source of errors is modifying object which has been deleted in the remote storage, in which case push will stop on that object, and the next pull will remove that object from local storage and no more problems will occur. Currently, the push sends all local state modifications to the server, without checking if we have observed the latest state of an object. For example, if we last synchronized half an hour ago, and we modify a comment, we will overwrite changes in that comment which occurred in that half hour period.

In the pull operation, we get all objects which exist on the server, and update local data based on that information.  If an object does not exist on the server anymore, it is removed from local storage.  If an object has been modified both in the local storage and the remote storage, the timestamps are compared and the latest modification is applied. 

Both push and pull can be done for individual object types and for all objects in the project scope.  In the event that an application performs an operations on an individual object, it needs to asure that all of the needed data is sent together – e.g. when synchronizing Comments or ToDos, the application must be be sure to synchronize Comments prior to synchronizing ToDos as Comments are dependent on the existence of an associated ToDo. 

### <a name="sync-client">Using the Sync Client</a>

The **Sync Client** provides an easy way to share the data stored in the local storage using TC services. This class represents a connection between local storage and cloud storage.

_SyncClient_ implements an _IRemoteStorage_ interface and can be used directly with the synchronization methods.

### <a name="pulling">Pulling objects from a remote storage system</a>

The _Pull()_ method reads the available entities from the remote storage system.  The method has an optional argument for a callback that is invoked for each pulled entity.

    await storage.PullAsync(remoteStorage, record =>
    {
        // the record was pulled from the remote storage system
    });

In similar way, storage.Todos.PullAsync() will pull ToDos from the remote storage.

### <a name="pushing">Pushing objects to a remote storage system</a>

The _Push()_ method sends the pending entities to the remote storage system. The method has an optional argument for a callback that is invoked for each pulled entity.

    await storage.PushAsync(remoteStorage, record =>
    {
        // the record was synchronized with the remote storage system
    });    

In similar way, storage.Todos.PushAsync() will push todos to the remote storage.

### <a name="conflicts">Conflict resolution</a>

The conflict resolution for all entities is fully automatic.  The logic currently implemented in TC SDK sync component is as follows:

1. If  there is a conflict (_modify-modify_ or _delete-modify_) detected when pulling data from the TC backend then the following logic is applied:

    1. Deletion always wins.  If an entity is deleted locally or remotely the delete status is preserved even if the entity is modified on the other side

    2. If deletion does not apply and win, then the latest timestamp wins, meaning that if the entity is modified locally later than in the cloud, the local modified state is preserved until the next time _PushAsync()_ is called

2. Upon pushing, currently there is no way to detect a conflict for entities.  This means that entities modified locally will override state of the object in the cloud.  In this sense it is advisable to call a _PullAsync()_ each time just before the PushAsync()

### <a name="snippets">Code snippets</a>

#### <a name="example-pull-project">Example: Pull cloud project to a new local storage instance</a>

If we have a cloud project and want to create corresponding local storage for it, we must create an instance of the _SyncClient_ class for the TC project as shown below.

    var serviceUri = ...;
    var client = new TrimbleConnectClient(serviceUri);
    
    await client.LoginAsync(token)
    var projects = await client.GetProjectsAsync();
    var project = projects.First(); // select a project somehow
    
    // get project specific wrapper
    var projectClient = await client.GetProjectClientAsync(project);
    
    // initialize the sync client
    var remoteStorage = new SyncClient(projectClient);
    
    // create a new local storage instance to cache project data    
    var path = project.Identifier;
    var storage = await remoteStorage.CreateStorageAsync(path);
    
    // pull data to the local storage    
    await storage.PullAsync(remoteStorage);

#### <a name=example-sync-existing">Example: Synchronize existing local storage with corresponding cloud project</a>

In the next scenario we have a local storage instance, and we would like to refresh its data from the cloud.  In this case, a connection is created following way:

    var serviceUri = ...;
    var client = new TrimbleConnectClient(serviceUri);
    
    var path = ; // path to existing local storage    
    var storage = new Storage(path); 
        
    // initialize the sync client from the existing local storage
    var remoteStorage = await SyncClient.CreateAsync(client, storage);   
        
    // now we can sync data    
    await storage.PullAsync(remoteStorage);

#### <a name=example-push-storage">Example: Create a cloud project from the local storage created offline</a>

In this scenario, we have a locally created project that we want to publish to the cloud.  Note that in this case the identifier of the project and potentially identifier of the current user, are changed after the push because they are assigned by the server. 

    var path = ; // path to existing local storage
    
    var user = new Person 
    {    
        Email = "idonot@exist.com"    
    };
    
    var project = new Project    
    {    
        Name = "Test project",    
        Location = "Europe", // must choose a real location here obtained through online API    
    };
    
    var storage = Storage.Create(path, user, project); 
        
    // initialize the sync client from the existing local storage
    var serviceUri = ...;
    var client = new TrimbleConnectClient(serviceUri);
    var remoteStorage = await SyncClient.CreateAsync(client, storage);
        
    // now we can sync data    
    await storage.PushAsync(remoteStorage);

#### <a name=example-initial-pull">Example: How to create a local storage for an existing cloud project</a>

In this example app, we have a list of cloud projects for the user and want to create correspondening local storage for the project (join the project) to store/cache data locally.

    var authResult = authCtx.AcquireToken(...);
    
    var project = (await trimbleClient.GetProjectsAsync()).First();
    
    var projectClient = await trimbleClient.GetProjectClientAsync(project);
    
    var remoteStorage = SyncClient.Create(projectClient);
    
    var projectFolder = Path.Combine(authResult.UserInfo.UniqueId, project.Identifier);
    
    using(var storage = await remoteStorage.CreateStorageAsync(projectFolder))    
    {    
        // start pulling content    
        await storage.Project.PullAsync(remoteStorage);    
        await storage.Persons.PullAsync(remoteStorage);    
        ...    
    }

### <a name=sharing-files">Sharing files</a>

The local data storage component provides an easy way to share files between multiple devices.  The sharing of file content is fully under the application's control.  This concept is different from object sharing however, as applications do not have a choice for syncing individual objects.

The file synchronization component is aware of model files (generated formats and attached alignments) and simplifies working with this information as a whole.

#### <a name=sync-files">Synchronizing files with a remote storage system</a>

The synchronization functionality is divided into three phases:
    1. Refresh the file entries from the remote storage system
    2. Resolve any conflicts between the local remote storage systems
    3. Push and pull files as indicated by their status codes

#### <a name=refresh-files">Refreshing file entries from a remote storage system</a>

The first step in synchronization is to retrieve the file entries from the remote storage system and update the file status.  This is done with the _Refresh()_ method in the _Files_ collection.  The method has an optional argument for a callback that is invoked for each retrieved file entry.

    await storage.Files.RefreshAsync(remoteStorage, file =>
    {
        // file entry was added or updated
    });

The _RefreshAsync()_ method synchronizes all metadata for the file structure and sets the statuses for file content.  It does not synchronize file contents.  All locally created folders are pushed to the cloud and all remotely created folders are pulled to local storage and accessible immediately from the _Files_ collection.  All file and folder renames and moves are also immediately synchronized.

In addition to files and folders metadata the _RefreshAsync()_ method will fetch the model alignment information and set appropriate statuses.

#### <a name=file-status">Checking the file status</a>

The _Status_ property in the file entry provides the file status from the local point of view.  The status is updated during the RefreshAsync() call.

The possible values for Status are:

* **Current**
    * The file is current and no action is needed.
* **Conflict**
    * The file has been modified both locally and remotely. The conflict cannot be resolved automatically, instead you must either delete the local file, pull the file from the remote storage system (overwriting the local file) or push the file to the remote storage system (overwriting the remote file). You may also attempt manual merge by copying the local file somewhere else, pull the file from the remote storage system and merge the file contents.
* **Pull**
    * The file has been added or modified remotely and is ready to be pulled.
* **Push**
    * The file has been added or modified locally and is ready to be pushed.
* **PullAlignment**
    * The file content has not been modified but there is a new alignment set remotely and ready for pull.
* **PushAlignment**
    * The file content has not been modified but there is a new alignment set locally and ready for push.
* **ConflictAlignment**
    * The file content has not been modified but there is a new alignment set both locally and remotely.  The conflict is resolved automatically using the latest timestamp when PullAsync() is next called.  If PushAsync() is called, local changes override any remote changes.
* **Deleted**
    * The file has been deleted remotely, but still exists locally.  Delete the local file entry.

Note that alignment related statuses (PullAlignment, PushAlignment, ConflictAlignment) are set only if file content is not changed (both remotely and locally).  If file content has changed, this change supersedes the alignment change.

Here is one way to implement the status check:

    switch (file.GetStatus())
    {
        case LocalFileStatus.Conflict:
            // file has been modified both locally and remotely
            break;
        case LocalFileStatus.Pull:
        case LocalFileStatus.PullAlgnment:
            // file is ready for pull
            break;
        case LocalFileStatus.Push:
        case LocalFileStatus.PushAlignment:
            // file is ready for push
            break;
        case LocalFileStatus.ConflictAlignment:
            // alignment has been modified both locally and remotely
            break;
        case LocalFileStatus.Deleted:
            // file has been deleted remotely
            break;
    }

#### <a name=file-conflicts">Resolving file conflicts</a>

In the event a file is modified by two users at the same time, a conflict may occur. 

Conflicting status is known after the RefreshAsync() method is called.  In this case, the file status will be set to _LocalFileStatus.Conflict_.  

After that, an app can decide how to resolve the conflict using the following call sequence: 

* _PullAsync()_ - this method overrides all local changes with the latest changes from server

* _Discard()_ - this method discards all local changes and revert the state to the last synchronization point

* _Rebase()_ - this method rebases changes done locally on top of the latest remote changes

Note that if PushAsync() method is called in a conflicting state, it will fail.  You must resolve conflicts prior to pushing.

Another type of conflict that may arise is occurs when a file or folder with the same name is added both locally and remotely (by another user). 

* This type of conflict is resolved automatically for folders by the SDK sync component during the RefreshAsync() call. 

* For files, the status will be set to _LocalFileStatus.Conflict_ and the app must decide how to resolve the conflict.

#### <a name=pulling-files">Pulling files from a remote storage system</a>

The Pull() method retrieves the latest version of the file from the remote storage system. To minimize the volume of the transferred data, do not call this method in case the local file status is set to _Current_.

    await file.PullAsync(remoteStorage, progress =>
    {
        // progress changed
    });

The Pull() method also pulls the latest alignment for the model if it is available from server.  Automatic conflict resolution based on timestamps applies in this scenario.

#### <a name=pushing-files">Pushing files to a remote storage system</a>

The Push() method sends the local version of the file to the remote storage system. To minimize the volume of transferred data, do not call this method in the case where the local file status is set to _Current_.

    await file.PushAsync(remoteStorage, progress =>
    {
        // progress changed
    });

Note that pushing a new locally added file will assign a new identifier to the file.  An app can use a _LocalFile.Id_ property.  This Id is a local identifier that is preserved during the push.  If new identifier is assigned for file that has an alignment the identifier for the alignment is also updated.

The Push() method also pushes the alignment for the new model version if it exists in the local storage.  Local alignment will override any alignment set in the cloud for a previous version.

#### <a name=generated-formats">Working with generated file formats</a>

The TC backend generates new file formats during file processing when the model file is uploaded.  An app can use the sync and local storage components to synchronize non-original content of the model files but generated formats.

The following methods accept a format specification as an optional parameter: GetStatus(format), GetPath(format), PullAsync(remoteStorage, format).

#### <a name=example-formats">Example: Initial creation of the local storage for the cloud project</a>

In this example, a local storage instance is created to store the data for an existing cloud project and then it is populated with existing data from the cloud.  For the model files (files with _.ifc_ extension) we pull _f3d_ content instead of the original content.

    var authCtx = new AuthenticationContext(...);
    var accessToken = await authCtx.AcquireTokenAsync(...);
    var client = new TrimbleConnectClient(ServiceUri);
    
    await client.LoginAsync(accessToken);
        
    // get project connection
    var projects = await client.GetProjectsAsync();
    var project = projects.First();
    var projectClient = await client.GetProjectClientAsync(project);
    
    // create a sync client for the project
    var remoteStorage = SyncClient.Create(projectClient);
    
    // create a new local storage
    var storagePath = Path.Combine(..., client.ProjectIdentifier);
    using (var storage = await remoteStorage.CreateStorageAsync(storagePath))
    {
        // populate members
        await storage.Persons.PullAsync(remoteStorage);
    
        // pull project descriptor
        await storage.Project.PullAsync(remoteStorage);
    
        // pull todos and comments
        await storage.Todos.PullAsync(remoteStorage);
        await storage.Comments.PullAsync<Todo>(remoteStorage);
    
        await storage.Comments.PushAsync(remoteStorage);
    
        // pull files
        await storage.Files.RefreshAsync(remoteStorage);
        foreach (var file in storage.Files.WithType(NodeType.File))
        {
            var isModel = file.Name.EndsWith(".ifc");
            switch (file.GetStatus(isModel ? FileFormat.F3D : FileFormat.Original))
            {
                case LocalFileStatus.Current:
                case LocalFileStatus.Push:
                    break;
    
                 case LocalFileStatus.Pull:
                    if (isModel)
                    {    
                       await client.WaitForProcessingCompletedAsync(
                           file.Identifier, format: FileFormat.F3D);
                    }    
                    await file.PullAsync(
                       remoteStorage, isModel ? FileFormat.F3D : FileFormat.Original);
    
                    var path = file.GetPath(isModel ? FileFormat.F3D : FileFormat.Original);
                    break;    
            }
        }    
        ...
    }

    public static async Task<int> WaitForProcessingCompletedAsync(
      this IProjectClient client,
      string fileId,
      string fileVersionId = null,
      string format = null)
    {
        var status = await client.Files.GetFileProcessingStatusAsync(
           fileId, fileVersionId, format);
        while (status >= 0 && status < 100)
        {
            await Task.Delay(100);
            status = await client.Files.GetFileProcessingStatusAsync(
              fileId, fileVersionId, format);
        }
        return status;
    }

### <a name=commands">Commands</a>

Certain operations cannot be represented as objects or files in the local storage system.  For such operations, local storage provides _command_ _queues_.  The command queues can contain any type of operations (as long as they are understood by the remote storage system) and maintain the order of operations.  Any number of queues can be used.

Commands are added to the queue with the _EnqueueCommand()_ method.  The first argument is the command, formatted as JSON. The second argument is the name of the command queue.

storage.EnqueueCommand("WipeData", "Setup");

The command is stored in the queue, but not immediately executed.  To execute the commands, use the _ExecuteCommands()_ method.

await storage.ExecuteAsync(remoteStorage, "Setup");

The method takes the remote storage system as the first argument and the name of the command queue as the second argument.
 After execution, the commands are removed from the queue.

You can see the pending commands in the _Commands_ collection.

    foreach (var command in storage.Commands.In("Setup"))
    {
        // use the command here
    }

The Commands collection supports query operations such as _In()_, _Count()_, and _Delete()_.

#### <a name=using-commands">Using command queues</a>

The primary use case for commands is to represent operations that cannot be performed in the local storage system alone. For example, you may want to invite other people to share your data.  Such invitations can be stored and later execute as a _command_.

Multiple command queues can be defined for ordering the commands into phases.  Usually, invitations should be sent after the data has been synchronized with the cloud.  However, certain setup operations may be needed before the data can be sent to the remote storage system.  For such cases, two command queues can be defined as follows:

    // enqueue setup operations
    storage.EnqueueCommand("CreateWorkspace", "Setup");
    
    // create the data
    storage.Save(...);
    
    // enqueue finalization operations
    storage.EnqueueCommand("InviteMembers", "Finalize");

Now the synchronization with the remote storage system can be implemented as follows.

    // execute the setup operations
    await storage.ExecuteAsync(remoteStorage, "Setup");
    
    // upload the data
    await storage.PushAsync(remoteStorage);
    
    // execute the finalization operations, including invitations
    await storage.ExecuteAsync(remoteStorage, "Finalize");

The command queue ensures that the operations are executed in the same order as they were enqueued.  This allows, for example, the application to create a new object in the remote storage system with one command and then apply some permissions on the object with another.

