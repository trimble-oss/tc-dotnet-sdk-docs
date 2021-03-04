# Trimble Connect .NET SDK Release Notes

*(Applicable for Trimble.Connect.Data and Trimble.Connect.Data.Sync components)*

# 2.7.8
* Fix: Added 'Center' column as last column in the table to fix errors during migration.

# 2.7.7
* Fix: Allow presentation format .trb to be uploaded even when back-end throws file type not supported error (for files like .nwd).

# 2.7.6
* Added 'Center' column to 'Clashes' table.
* Modified the Model Elements identifier to be based out of source identifier and file version identifier to align with the TCPS changes.

# 2.7.5-beta
* Fix: Fetch placement for known file version identifier.

# 2.7.4-beta
* Fix: Added revision number to placements table to fetch placements using file id and unknown version ids.

# 2.7.3-beta
* Integrated new pre-release client versions (Trimble.Connect.Client, PSet client, SyncHelperClient).

# 2.7.2-beta
* Marking pre-release with beta tag as it depends on pre-release packages.

# 2.7.1
* Using portable debug symbols.

# 2.7.0
* Integrated new pre-release client versions (Trimble.Connect.Client, PSet client, SyncHelperClient).
* Use Trimble.Connect.Client (v2.6.2-beta or above) and initialize the TrimbleConnectClient with ICredentailsProvider.
* Updated the minimum support version for uwp to 10.0.16299 to be compatible with net standard 2.0 clients.

# 2.6.25
* Fixed bugs and vulnerabilities reported by sonarqube and whitesource.

# 2.6.24
* Fix: Persisting the folder identifier for parent identifier of subfolders.

# 2.6.23
* Fix: Pulling files using IFiles.PullLatestSnapshotAsync persisted the version identifier for parent identifer in File Versions Table.
* Data migration and code fix done to persist the file identifier for parent identifier in file version table. 

# 2.6.22
* Fix: Files sync using IFiles.PullLatestSnapshotAsync should not mark files being pushed while pulling as deleted.

# 2.6.21
* Fix: PullChildrenTs will not be set for root on IFiles.PullLatestSnapshotAsync that leads to errors in file sync on clearing storage.
* Fix: Pull files / folders with only readonly change should persist the readonly in storage.

# 2.6.20
* Fix: Pull file permissions to local storage. 
* Added IFiles.PullLatestSnapshotAsync to get the latest server snapshot to local.
* Provided StorageOptions.PullFilesFromZeroShouldIncludeVersions for backward compatibility to pull file versions everytime. Default is false and first time pull for files will skip versions.

# 2.6.19
* Fix: Trimble.Connect.Data.Sync component will directly use Trimble.Connect.PSet.Client to get and set PSets for quick access items instead of using Trimble.Connect.Client component.

# 2.6.18
* Fix: Folder deleted issue when using hybrid approach (PullChildrenAsync and PullAsync) for pulling files and folders.

# 2.6.17
* Fix: Views with segments markup cannot be pushed.

# 2.6.16
* Added fileset and pathtemplate parameters to Upload and Download APIs to support point cloud files.
* Added fileset and pathtemplate to FileTransferOptions that is used in most of the upload/download APIs.

# 2.6.15
* Fix: Skip file assimilation status checks while pushing presentation format .trb
* Added IViews.GetViewContentPath() to get the view json file path. Applicable only for large views.

# 2.6.14
* Fix: Issues in parsing large views in Markups and Element states.

# 2.6.13
* Fix: Error while pushing large views.

# 2.6.12
* Added IViews.GetView() methods with a flag to optimize loading large view details on demand. 

# 2.6.11
* Added support for pulling large views by skipping the large view details while pulling views. The large views should be pulled separately one by one using their identifier already synced.
* The large view data will be stored in file cache to optimize the DB.
* Added exception while deleting view2D attached to todo.

# 2.6.10
* Fix: Multiple alignments will be synced for a model version and the latest alignment will be used.

# 2.6.9
* Added support for multipart download.
* Multipart download will be applied when FileTransferOptions.UseSegmentedTransfer is set to true and the file to be downloaded is of size greater than 50 MB.

# 2.6.8
* Fix: Deleted entities will not be pushed.

# 2.6.7
* Added support for syncing quick access items. 

# 2.6.6
* Fix: Removed users in created by, modified by and assignee list of entities are rightly synced.

# 2.6.5
* Added support for pulling Single File Entity. 

# 2.6.4
* Fix: File attached to release should not be deleted.

# 2.6.3
* Adapted to the single step authentication mechanism provided by Trimble.Connect.Client v2.4.4
* Added support for downloading 2D files as pdf file format.

# 2.6.2
* Fix: Set IsDeleted flag in entity descriptor.

# 2.6.1
# 2.6.0
* Integrated data ocean file services API
* Added support for links with embedded file target.

# 2.4.46
* Fix: Persons API should not fetch deleted persons

# 2.4.45
# 2.5.44
* Fix: GetPage API should set the 'Assigned To' property for entities.

# 2.5.43
* Fix: GetPage API updated not to fetch embedded entities.

# 2.5.42
* Updated the migration scripts to be compatible with latest SQLite version 3.27.1 used in Trimble.SQLite v 1.0.61 and above.
* Note: If previous versions of this component is used with Trimble.SQLite v.1.0.61 or above, migrations could fail

# 2.5.41
* Added exception while deleting view attached to todo.

# 2.5.40
* Updated the copyright information.

# 2.5.39
* Fix: View assigned to usergroup is unassigned to user on deleting the user group on the next views sync.

# 2.5.38
* Fix: GetPage API updated not to fetch deleted entities. 
* Fix: Issues in GetPage API with multi column search.

# 2.5.37
* Fix: Links updated with new source elements replace the old source elements.

# 2.5.36
* Fix: Embedded attachment reappearing in storage when unlinked during push.

# 2.5.35
* Fix: Project settings synchronization issue.

# 2.5.33
* Added multiple sort functionality to GetPage API for LocalFile.

# 2.5.32
* Fixed issue in new storage object creation from old storage.

# 2.5.31
* Added search functionality to entity types views, todos and persons in GetPage API. 
  -The search string for views filters the views based on view name or description.
  -The search string for todos filters the todos based on todo title or text.
  -The search string for persons filters the users based on first name or last name.

# 2.5.30
* Added search functionality to entity type LocalFile in GetPage API. The GetPage API takes a search string that is a full or partial search key.

# 2.5.29
* Added Companies table that persists the company names. 
* Added Companies column in Persons table that references the companies that the user belongs to.

# 2.5.28
* Fix: Cache the time stamp of the last pull children call for the folders and use it to mark the old versions of deleted files/folders as deleted during the files pull async.
* This fixes the issue of showing deleted files and folders during the sync.
* Added ignored items list to PullChildrenAsync call.

# 2.5.27
* Fix: Updated parent folder modified time and size on PullChildrenAsync call and updated the progress call back.

# 2.5.26
* Added new storage options to specifiy page size for entity types to fetch from local storage.

# 2.5.25
* Fix: DatabaseSchemaCorruptedException might be thrown on storage opening if v76 storage is open concurrently from several threads even if storage is not actually corrupted. Transactional locking added to db schema migration code.

# 2.5.24
* Added LastAccessed, timezone, phone number properties to person entity type and persisted them in local storage.

# 2.5.22
* Fix: Updating parent(folder) size on PullChildrenAsync call.

# 2.5.21
* Fix: Database query fix for updating removed file attachment versions.

# 2.5.20
* Fix: Updating removed file attachment versions.

# 2.5.19
* Added custom sort order for todo priority/status in paged requests for Todo as per the listed order in TodoPriority/TodoStatus classes.

# 2.5.18
* Added sort by type(file/folder) for entity type LocalFile in GetPage API

# 2.5.17
* Added support for querying local storage with paged requests with optional sort property.

# 2.5.16
* Added new storage options to specifiy page size for entity types and db timeout.
* Fix: Corrected DB transaction exception while undeleting embedded file.

# 2.5.15
* Fix: Reattaching the same file after deleting the attachment

# 2.5.14
* Fix database deadlock while pushing model placements if there are more than one placement to push.
* Fix: Missing transaction added while pulling placements.
* Fix: use separate storage connection from background thread while pushing file resentation formats.

# 2.5.13
* Improved file sync performance by combining file and folder streams using FsObject sync.

# 2.5.12
* Improved the pull strategy by storing the embedded entities as soon as they were discovered.

# 2.5.11
* Fix: Fixed the internal issue to improve the robustness.

# 2.5.10
* Fix: database schema conversion bug introduced in 2.5.9 - coverter v78 -> v79 is missing

# 2.5.9
* Fix: File attachment thumbnailsUrl are updated on assimilation events.

# 2.5.8
* Fix: Fixed storage state for local deleted entities

# 2.5.7
* Improvement: avoid pulling file thumbnail twice by send tokenThumburl parameter when pulling folder items with IFiles.PullChildrenAsync() when moving or renaming a file.

# 2.5.6
* Fix: occasional db lock on pulling entities.

# 2.5.5
* Fix: In case of delayed response from server while pushing an entity, local entity changes are persisted in storage.

# 2.5.4
* Fix: Override option for pushing presentation format does not work in some cases.

# 2.5.3
* Fix: The DatabaseSchemaCorruptedException.Project.RootFolderIdentifier is null. This does not allow to recreate the storage using Storage.CreateFromRemoteState().

# 2.5.2
* Fix: on opening existing db don't execute the "PRAGMA journal_mode=WAL" each time, since the WAL mode is persistant execiute it only on db creation. This improves the db opening performance and solves the rare "SQLite error (5): database is locked" issue on concurrent opening of the same db.

# 2.5.1
* IFiles.RefreshAsync() obsolete method has been removed
* ResetSyncCursor() obsolete method has been removed
* IPushableRepository<T>.ResetAsync() obsolete method has been removed
* Support for snapshot based sync for files has been removed
* Fixed locking when pushing files and folders: file/folders might have been pushed twice if app calls several push operations in parallel.
* Improved parallelizm when pushing and pulling items in parallel (e.g. previosly push was blocked until pull is fully completed, witch might be very long operation for initial project join scenario)
* Storage.Options property is exposed in IStorage interface
* IFiles.PullChildrenAsync() method has been introduced to help with initial folder structure sync scenarios for large projects. This call prioritize the sync of the content (one level) of the specific folder over backgroud full sync.

# 2.5.0
* Incompartible change: ViewGroup.Views property has been removed and ViewGroup.ViewIdentifiers property has been added instead (db schema change v76->v77).
  - Ability to pull ViewGroups without (or before) pulling the Views to local storage.
  - ViewGroups can refer to views that are not accessible to the user at the time of pulling ViewGroup, this reference will not be lost any more. So if user got access to the view the reference is there to use.
  - App has a information regarding whether ViewGroup is empty or user just has no access to views in the group. This way app can make informed decisions on UX interaction level, e.g. whether to allow user to delete a ViewGroup or suggest user to request access to views as well as do proper visualization for reordering operations.

# 2.4.10
* View permission deny event is taken in use: when user removed from view assignees the view will be marked as deleted in local storage on next views pull.

# 2.4.9
* Thumbnail management for files has been optimized to avoid downloading same thumbnail twice after file upload (tokenThumburl=false parameter is used).

# 2.4.8
* Added automatic recovery for the corrupted databases with local only data (if data has been synced with server we reject to open the database and reporting DatabaseSchemaCorruptedException), in previous version the DatabaseSchemaCorruptedException was thrown for local only storages as well, so user was losing data.

# 2.4.7
* fix for database schema migration bug introduced in 2.3.17 (db schema conversion v70->v71): 
  - conversion is fixed, 
  - DatabaseSchemaCorruptedException exception is thrown on opening db if corrupted db is detected (passing the db schema version and project desriptor). The recommended recovery procedure is to delete the corrupted database and recreate it again.

# 2.4.5
* Fix: error on pulling files from the empty project (when folder cursor == null)
* Fix documentation for the LocalFile.AddPresentationFormatAsync
* Fix documentation for the LocalFile.GetContentPath FileVersion.LocalFile and LocalFileExtensions.GetLatestDownloadedContentPath
* View groups are using delta based sync by default now.

# 2.4.4
* Optimize IFiles.PullAssimilationChangesAsync in case of initial project join
* Prevent links which refer to local only items from being pushed

# 2.4.3
* Added UpdateFromRemoteState extension to enable update project descriptor with information got from listing remote projects. Previously app had to call IProjects.PullAsync() which generated additional network call.

# 2.4.2
* Fix: Progress callback for the IFiles.PullAssimilationChangesAsync implemented
* Fix: page size can be configured when fetching assimilation events using IFiles.PullAssimilationChangesAsync

# 2.4.1
* Added LocalFile.SchedulePresentationFormatUpload() and FileVersion.SchedulePresentationFormatUpload() methods to schedule presentation file uploads. 
  Can be used if the presentation file is already in the file cache.
* Added ability to override the presentation file format that exists on the server by uploading a new content for presentation file.
* Incompartible change: AddPresentationFormatAsync methods don't schedule the upload automatically any more, SchedulePresentationFormatUpload should be called explicitly

# 2.4.0
* Renamed DiskUsageOptions to DiskUsageType

# 2.3.31
* Added IStorage.DiskUsage() to calculate disk space used for the local cache
* ClearOptions for the IStorage.Clear() method are improved, some options are depricated (Unchanged, Modified, NotReferencedFileContent, FileHistory) and some new are introduced (HistoryFileContent)

# 2.3.30
* Fix peformance bug introduced in 2.3.21 (db schema v73): files metadata pull becomes slow for large file structures (same as was fixed in 2.3.9).
* Incompartible change: IFiles.PullAssimilationChangesAsync method is split out of IFiles.PullAsync. As a result thumbnail and triangleCount changes are not pulled as part of IFIles.PullAsync any more.

# 2.3.29
* Fix: local undelete operation should always win on conflict resolution. Before the fix after the file sync cursor reset locally undeleted folders were marked back as deleted again.
* Fix: Allow to attach several versions of the same file to todo (db schema is incremeted to v75)
* Fix: Links to file (todo and comment attachments, alignments) are automatically patched with assigned (by server) file version identifier after file push, if they were referring to locally modified file.

# 2.3.28
* Fix: cannot add embedded file attachment if there was an attachment (now deleted) with the same name already

# 2.3.27
* A workaround for missing source type information in links created by the TC WEB

# 2.3.26
* Adds triangle count to *LocalFile* and *FileVersion* as read-only property which is synchronized from the backend
* Fixes issue when generated thumbnail wouldn't be pulled if file was pulled before thumbnail was generated

# 2.3.25
* Improves performance of querying files by parent folder or name
* Fixes issue with project settings not being pulled in some scenarios if storage was created by the SyncClient.CreateStorage() method (introduced in 2.3.23)

# 2.3.24
* Fixes 65 to 66 schema migration (SDK version 2.3.1) to preserve hashes of newly added files

# 2.3.23
* SyncClient.CreateStorage() method added to create a storage out of the existing project descriptor (e.g. in scenario when listing projects)
* IStorage.Clone() method added as a alternative to copy constructor. This method allowes to create new connections to existing storage without knowing exact type of the storage
* Fixed the LocalFile.Size property after the file update operation

# 2.3.22
* Add *LastSynchronized* to repositories which stores time of last performed pull for an entity type
* Modify *Clashes* so we don't ignore clashes belonging to a deleted *ClashSet* 

# 2.3.21
* Fix database constraint that was preventing some file operations because of duplicate name if there are files and/or folders marked as deleted in the same folder

# 2.3.20
* Prevent thumbnails from being downloaded multiple times on parallel calls of *IPullableThumbnails.PullAsync{T}*
* Fix sync bugs introduced in 2.3.19

# 2.3.19
* Added *Discard* method to **ClashSets, Comments, Links, Placements, Releases, Tags, Todos, ViewGroups, Views**. This method discards local changes and revers data back to latest known remote state.
* As a consequence of adding *Discard*, *IPushableRepository.ResetAsync* is marked as obsolete because *Discard* covers the needed functionality 

# 2.3.17
* Fixes bug preventing users and group synchronization if there is a user and a group with same identifier
* License text updated: https://community.trimble.com/docs/DOC-10021 
* Upgrade dependencies so they point to packages with correct license

# 2.3.16
* Added 'LocalFile.UpdateAsync(Stream)' method. 

# 2.3.15
* Fix: file download blocks the file metadata sync. 
* Change has been made to 'LocalFile.PullAsync()' logic: this method does not update the file metadata any more even if there is a newer version of the file available on the server, but just download the content for whatever version is known locally.

# 2.3.14
* API and db schema (v70) change to improve forward compartibility: 'TodoStatus' and 'TodoPriority' are string literals now instead of enums.
* Parallelism is improved when consuming paged changes from server and applying them to local storage. 

# 2.3.14-hotfix
* A hotfix for TCM v2.2 release that includes all performance improvements and fixes from 2.3.14 and 2.3.15 except the 'TodoStatus' and 'TodoPriority' change.

# 2.3.13
* Improvement: give a single callback on a snapshot based sync instead of one per each change detected. Give also a total count of entities received in the snapshot instead of null in a total count.
* Fix: HT was not used to guard the shapshot based sync
* Added: 'LocalFile.GetFullPath' method
* Fix: Deleted entities are missing in 'SyncProgressEventArgs' progress notification on pull

# 2.3.12
* The sync component version is sent as part of the UserAgent header

# 2.3.11
* Add support for adding presentation formats of files to the file cache and pushing them to remote storage

# 2.3.10
* Bugfix: File sync cursor might be corrupted sometimes if sync is interrupted

# 2.3.9
* Performance improvements for file metadata pull
* Bugfix: Scenario in which after pull remotely deleted folders are incorrectly undeleted locally

# 2.3.8
* File purge events are recognized and ignored

# 2.3.7
* File and folder versions sync is modified based on backend API changes (useVersions parameter)
* Fix: when reporting progress for delta sync the total number of items left to consume (SyncProgressEventArgs<T>.TotalCount) was not reported correctly
* PCL target is removed
* Fix: strong names for iOS and Android assemblies

# 2.3.6
* Fix: db initialization script is failing on Android < 23 (6.0) and earlier versions.

# 2.3.5
* Fix: local storage migration from 2.2.13 (schema v62) to v2.2.14 (schema v63)

# 2.3.4
* File and folder sync logic is improved to be compatible and to benefit from receiving all file and folder versions from server
* Conflict resolution for files and folders has changed: name and folder location changes are applied with the same rules as changes for other entities - latest timestamp wins; local content changes are never overriden, but always rebased on top of remote changes if any.
* When pulling file and folder structure the changes are applied to local cache immediatly, so user see up-to-date folder structure and names always.
	This new logic causes the difference in 'LocalFile.GetPath()' behavior: this method will not return path to previosly downloaded content if there is a newer file version available.
	If application wants user to work with some specific version of the file it should remember this version somewhere and ask for the content path for that specific version: 'FileVersion.GetPath()'
* 'LocalFile.GetPath()' and 'FileVersion.GetPath()' methods are renamed to 'GetContentPath()' to emphasize the logical change in the LocalFileBehavior. The 'GetLatestDownloadedContentPath()' extension method added to emulate the old GetPath() behavior.
* Fix: regression after 2.3.1 - LocalFile.Version should be null if file is modified locally
* Entity.Parent property is exposed

# 2.3.3
* Added sync support for project settings. Settings are synced together with project's descriptor.
* Incompatible change: Unit settings properties removed from Project entity and encapsulated in UnitSettings class which is part of ProjectSettings class used as Settings property of Project entity

# 2.3.2
* Obsolete 'ResetCursor' method has been removed
* Obsolete 'IPullableComments.PullAsync<TEntity>' method has been removed
* Changed the push/pull callback signature to use the IProgress interface
* Changed signature for 'IPullableComments.PullAsync<TEntity>(entity)' to return collection of comments for entity instead of accepting the callback
* Added IPullableThumbnails interface. Application is reponsible on pulling thumbnails explicitly now. Previously we were pulling them implicitly as part of other pull operations for previewable entities. This gives an option to application to manage thumbnail caching by its own.
* The 'IFiles.PullThumbnailsAsync' has been removed. 'IPullableThumbnails.PullAsync<FileVersion>()' to be used instead.
* New option is introduced that can be set by app in runtime ('IStorage.Options') or via configuration file ('.TrimbleConnect\.config'): 'PullChangesPageSize' (default is 100).

# 2.3.1
* Tracing added with Trimble.Diagnostics package
* Transactional page processing for the sync activity added: sync streams are consumed and applied to local storage page by page.
* Fix: IRepository.Get(long) returns null when 0 is passed to be consistent with behavior that null is returned for unknown entity
* File storage structure is refactored to be more compact
* Deleted file version is now stored with information about who and when has marked file as deleted
* Descriptors for embedded files are pulled as part of the normal stream (IFiles.PullEmbeddedAsync method is removed)

# 2.3.0
* **WARNING** During local storage migration the Placements repository will be cleared. Use Files.RefreshAsync or Placements.PullAsync to restore them from the online service 
* Add support for delta based sync of placements (enabled by default)
* Breaking change: Previously pulling placements only resulted in changing the placement's remote status, and individual placement needed to be pulled in order to refresh data. Now placements are automatically pulled and potential conflicts are resolved as for other entities - deletion always wins, otherwise last modification wins. The LocalFile.GetStatus() will not return statuses PullPlacement and ConflictPlacement anymore. PushPlacement is still used. 
* Use placement inheritance logic rules to find applicable placement for a file version
* Change: If all placements have Deleted status, IPlacements.GetByFile will return null
* Breaking change: FileId removed from Placement and Alignment, FileIdentifier and FileVersion are used instead
* Alignment class marked as obsolete, will be removed in the future
* Fix: Links cursor was throwing error on Reset and wasn't used in actual delta requests

# 2.2.15
* Fix: Return link query results correctly when the queried object descriptor has correct local identifier and placeholder global identifier
* Fix: Updating link uses correct global identifier even if the node descriptors are referring to placeholder global identifiers

# 2.2.14
* Added support for url names.

# 2.2.13
* Added ´UpdatedBy´ and ´Updated properties to project stats to reflect last known content modification which happened in the cloud.

# 2.2.12
* Bugfix for embedded file attachments to comments

# 2.2.11
* Bugfix for links data schema

# 2.2.10
* Fix: Use correct parent folder and name information in file queries (fixes evaluation of LocalFile.Children and queries using IFiles.WithParentId)

# 2.2.9
* Add CRUD support for entity links
* Add sync support for entity links
* The ´IFiles.RefreshAsync´ operation is split to several phases that can be invoked separatelly by app.

# 2.2.8
* ´IPullableComments.PullAsync<T>(..)´ method is marked as obsolete. Pulling comments for specific entity type is not supported any more. Instead all comments are pulled always.

# 2.2.7
* Improvement: The ´IReadOnlyRepository´ interface is extended with ´LoadAllPropertiesOnEnumeration´ property and ´Load´ method.
    The ´LoadAllPropertiesOnEnumeration´ property allows to control whether all properties should be loaded when enumerating collection or only minimum set of properties. This allows app to do performance optimizations and skip loading expensive properties when showing entities in lists in UI.
	The ´Load´ method allows to reload entity's properties in place without instantiating a new instance of the entity.
	The ´Views´ (´ElementStates´, ´ClippingPlanes´, ´Markups´, ´Camera´, ´IndicatorTypes´) and ´Views2D´ (´Markups´) collections support partial loading of entities on enumeration at the moment.

# 2.2.6
* Added: Support URLs as attachments.

# 2.2.5
* Added: Attachments support for comments.
* Change: Database are constraints relaxed to allow deletion of all entities (e.g. views) that are attached to ToDos and Comments (db schema change 59->60).

# 2.2.4
* Improvement: Added ´PushAsync´ and ´PullAsync´ operations to ´IFiles´ interface. Comparing to ´IFiles.RefreshAsync´ these operations are lower level and allow to implement one way synchronization.
* Fix: occasional db locked sqlite exceptions on pushing entities. SQLite is switched to WAL mode.

# 2.2.3
* Delta sync is enabled for 3d views by default

# 2.2.2
* Fix: don't try to push changes for files known to be changed remotely bacause it causes unnecessary failed requests.
* Fix: Automatic conflict resolution in case file is moved or renamed remotely but content is changed locally. Previously local changes were lost, now they are rebased.
* Fix: attachments that refer to local only entity are deleted automatically when the local entity is deleted
* Improvement: automatically recover the local state if server rejects the change request because of the state conflict (file rename or move)
* Improvement: Expose IFiles.SnapshotSync property
* Rename UseSnapshotSync to SnapshotSync property for all repos

# 2.2.1
* Constraints for link deletion changed: it is now allowed to delete linked entities.
	- for local only entities this will result in automatic deletion of all links for that Entity
	- for synced entities this will mark the entity as deleted and try to push this operation to the server on next ´PushAsync()´. 
	Server may reject this operation for some entity types (e.g. currently for files referred to from ´Release´s), but allow for others (e.g. files as attachments, files referred to from views, 2d views and clashsets).
	- for applications this change means that ´IAttachments.GetTargets()´ may return links that point to deleted entities.
* Added: ´EntityDescriptor.IsDeleted´ flag to handle scenarios when link points to deleted entity
* Local database constrants relaxed to support following cases:
	- deleted file attachments - file can be marked as deleted even if it is attached to a ´ToDo´
	- deleted file in a view - file can be marked as deleted even if it referred to from a ´View´
	- deleted file in a clashset - file can be marked as deleted even if it referred to from a ´ClashSet´
* db schema version incremented to v58
* Fix: LocalFile.Discard() operation also discards the delete local status now
* Improvement: deleted entities are not wiped from local storage automatically sometimes, but explicit IStorage.Clear() call is required always. Calling Delete() twice does not wipe entity/file any more.

# 2.2.0
* Added: Configuration framework introduced. 
 - Configuration can be supplied via: a) ´.TrimbleConnect\.config´ file; b) in runtime when constructing storage; c) to individual Push/Pull methods.
 - Following parameters can be configured (´StorageOptions´): ´MaxEntitiesToPushInParallel´, ´UseSnapshotPullFor´, ´FileCachePath´, ´UploadOptions´, ´DownloadOptions´
 - Repositories expose ´MaxEntitiesToPushInParallel´ and ´UseSnapshotSync´ properties
* Fix: ´ICommands.In()´ method returns a new scoped instance of the commands repository
* API change: All file transfer methods (PushAsync/PullAsync) accept ´FileTransferOptions´ as a parameter instead of separate buffer size and timeout arguments.
* Added: support for segmented resumable file content push with parallelism
* Added: ´IStorage.DirectoryPath´ property exposed
* The default stream activity timeout when transferring files has been changed to 2 minutes (was infinite previosly).

# 2.1.49
* Fix: cannot delete entity which has tags  

# 2.1.48
* Fix file sync so it's robust when receiving folder items out of order

# 2.1.47
* Fixes a minor internal issue

# 2.1.46
* Fix tagged objects sync deserialization of thumbnail uri's to handle array structure

# 2.1.45
* Fix sync logic so that after a sync cursor reset, entities to which access has been lost will be deleted on next pull

# 2.1.44
* Fix: pull thumbnail on indivudual file pull (LocalFile.PullAsync) if missed.

# 2.1.43
* Support the change in the ToDo permission model inroduced in the TCPS 2.50: ToDos are not public for project members any more.

# 2.1.42
* Optimize sync support for 2D views
* Expose EntityDescriptor.IsEmbedded property

# 2.1.41
* Add embedded file attachments support (db schema version changed 55->56)
* FileVersion.ParentType property added
* Files.Delete() query added

# 2.1.40
* Optimize sync support for ClashSets
* Fix: running pull and push in parallel will result in duplicated entities

# 2.1.39
* Add sync support for Tags

# 2.1.38
* Fix: missed methods added to created embedded 2D Views
* Fix: missed logic implemented to pull and push 2D views as attachments

# 2.1.37
* Bugfix for deletion of local only Views during Pull operation (also affected are ViewGroups, ClashSets, 2D Views)

# 2.1.36
* Support sync for 2D view

# 2.1.35
* Add automatic pulling of thumbnails for LocalFile and FileVersion entities when the Files collection is refreshed

# 2.1.34
* Bugfix for schema update in version 2.1.31.

# 2.1.33
* Bugfix for ITags.GetEntityTags to return only tags and not other entity links.

# 2.1.32
* Add local storage support for tags and tag operation. No sync capabilities.

# 2.1.31
* Added: 2D views support

# 2.1.30
* Improvements in handling cancellations when adding and updating files in local storage

# 2.1.29
* Bugfix: Cannot pull files and versions in parallel if they have same content

# 2.1.28
* Internal pull optimisation for files and todo sync
* Add ThumbnailUrl and Revision support to Files and FileVersions

# 2.1.27
* Bugfix: File pull progress percentage is calculated wrongly when not original file format is requested.

# 2.1.26
* Bugfix: IStorage.Clear(ClearOptions.FileHistory) does not clear the file history.

# 2.1.25
* Bugfix: LocalFile.Version should be null if file is modified locally
* Bugfix: LocalFile.Discard() does not reset changes in name and parent folder

# 2.1.24
* Bugfix: On mobile targets cannot create a Storage instance because a file cache is not found.

# 2.1.23
* Bugfix: view.AssignedTo is not updated when IViews.Update(view, thumbnailData, mediaType) overload is used.

# 2.1.22
* PullWithTimeoutAsync and PushWithTimeoutAsync extension method introduced in 2.1.21 are removed. Instead a LocalFile.PullAsync and PushAsync methods are added with timeout and bufferSize parameters.
* Added possibility to specify custom transfer buffer size for file push and pull.
* Added FileVersion.PullAsync with timeout and transfer buffer size parameters

# 2.1.21
* Extensions are added (PullWithTimeoutAsync, PushWithTimeoutAsync) to allow to pull and push file content with timeout on a stream activity.

# 2.1.20
* Predefined Todo types are removed

# 2.1.19
* Added: Support for Todo.PercentageCompleted property in local storage (db schema version changed 51->52). All locally cached todos are marked as dirty on upgrade, so next pull operation should populate new properties from the server.
* Fix: ResetSyncCursorAsync method missed for Files collection

# 2.1.18
* Added: ResetSyncCursorAsync method added (ResetSyncCursor is marked as obsolete) that allows to reset the sync cursor more reliably when async pull operations are running at the same time.

# 2.1.17
* Added: Support for Todo.Type and Title properties in local storage (db schema version changed 50->51). All locally cached todos are marked as dirty on upgrade, so next pull operation should populate new properties from the server.

# 2.1.16
* Improvement: Use pagination (1000 items per page) when pulling clash items to avoid long requests failing with timeout

# 2.1.15
* Fix: CurrentUserIdentifier is not updated on local project push

# 2.1.14
* Fix: missed locking on project push might cause duplicate project creation

# 2.1.13
* Fix database connection management: all asynchronous operations create a new separate connection to database for the operation.
This way app can continue to use normal CRUD operations with the storage instance that the async operation was initiates with while the async operation is running.
Note that app should still take care that synchronous operations for same Storage instance are NOT executed concurrently from different threads.

# 2.1.12
* Synchronization methods (push, pull, refresh) are now thread safe. It is ok for the app to call them concurerntly from diferent thread, Sync component with do the nessesery coordination automatically to avoid data corruption.
Previously it was up to the application to make sure only other thread is initiating the synchronization activity. Typical problem was a duplicate entities creation.
* Push parallelizm is improved. If there are many entities up to 10 parallel requests will be send to backend to improve performance.

# 2.1.11-beta
* Fix: automatic on demand file cache migration is removed to avoid problems related to atomicity of this operation and performance hit on storage construction. App could do migration as an admin action if needed.
* More intelligent file cache implemented that supports both local and shared stages simultaneously for the backward compartibility: files that are already in the local cache will be used from there, all new files are put to shared cache.
* App can now access and modify the file cache location using `IStorage.FileCache.DirectoryPath` property. App should take care not to change the file cache location when there are not published files because they cannot be restored from the cloud, or app should copy such files to a new location.

# 2.1.10-beta
* Fix: storage schema (48->49) cannot be converted if there are not published files

# 2.1.9-beta
* Fix: too much noise to trace console when sdk config file is missed 

# 2.1.8-beta
* Compatibility release: Update Sync component to work with Trimble.Connect.Client 2.0.290 and above

# 2.1.7-beta
* Added: Support for Releases in local storage

# 2.1.6-beta
* Bugfix: db schema v49 conversion fixed

# 2.1.5-beta
* Bugfix: db schema v49 conversion fixed

# 2.1.4-beta
* File history support added (FileVersions collection and LocalFile.Versions property). 
At this point we don't guarantee the full file history is available locally automatically- there could be some versions missed. 
But each version seen will be recorded and always available later even if file is updated or deleted. App can force the full file history retrival with FileVersions.PullAsync().
Added CleanOptions.FileHistory.
* db schema has changed (v49)

# 2.1.3-beta
* Add support for syncing project members' avatars. 

# 2.1.2-beta
* Shared file content caching support. 
Now the shared file content cache is the default behaviour on desktop platforms. 
The default location for the shared file cache is local user profile (`%USERPROFILE%\.TrimbleConnect\.files`). This location can be overriden using _FileCachePath_ parameter in the configuration file `%USERPROFILE%\.TrimbleConnect\.config`.
If relative file is specified in the configuration file then the file cache will be local for the project storage (same as behaviour in earlier versions).
On mobile platforms behaviour is unchanged.
When storage created with older SDK version is opened on desktop platform the SDK will look for the ".files" subfolder. If it exists automatic migration will be attempted to copy all the currently cached files to the shared cache. 
* In case of the shared file content cache the `Storage.Clear()` method behaviour has changed: files are never removed from the shared cache
* Added possibility to override the file content cache (blob storage) location on project storage creation, the BlobStorage class is exposed for this purposes
* Added callback on storage opening that informs app that the storage is going to be converted from older version and allows to cancel the conversion.

# 2.1.1-beta
* Comments pull optimization - pulling only comment changes since last synchronization
* Added functionality to pull all comments regardless of the related-to entity type
* Comments are soft-linked, the related-to entity does not need to exist in the storage  

======================
Trimble.Connect.Client has been moved to separate repo.
Notes below this line are applicable for Trimble.Connect.Client, Trimble.Connect.Data, Trimble.Connect.Data.Sync components.
======================

# 2.0.285

* Fix: undelete operation should be applied to parent folders as well

# 2.0.284

* license url in the package descriptor is updated to use new support.connect.trimble.com hostname
* The `X-TrimbleConnect-Client` header is introduced

# 2.0.283

* license url in the package descriptor is fixed

# 2.0.282

* Fix: the license information in local project descriptor is not updated in some cases on `PullAsync()`.

# 2.0.281

* `X-TC-INSTALLATION-ID` header is introduced. The unique installation identifier is generated for each application installation and upgrade. This feature is a foundation for the per app installation TC service API usage analytics and billing.
* forward compatibility improvements for date properties (`ToDo.DueDate`, `Release.DueDate`, `Project.StartDate`, `Project.EndDate`) values parsing in TC service responses. Now both `yyyy'-'MM'-'dd` and `yyyy'-'MM'-'dd'T'HH':'mm':'ssK` formats are supported.
* Fixed the UTC datetime conversion when processing date properties (`ToDo.DueDate`, `Release.DueDate`, `Project.StartDate`, `Project.EndDate`)
* `ITrimbleConnectClient.LogoutAsync()` method is added

# 2.0.280

* TotalLength for paged results calculation improved in case of empty response: returns 0 now instead of null.

# 2.0.279-beta

* Fix for ObjectDisposedException in interactive profile completion
* Upgrade dependency to Trimble.WebUI 1.0.3

# 2.0.278-beta

* Fixed login problem with interactive profile completion. With embedded view login have to be run in UI thread.

# 2.0.277-beta

* Migrated to Trimble.WebUI to support web UI flow
* mark API key based login as obsolete
* LoginAsync() method is extended with the profile completion flow with Web UI support

# 2.0.276

* Portable project is switched from Profile7 to Profile111 to prepare ground for netstandard11
* UWP target platform added

# 2.0.275

* ECOM_ENTITLEMENT_DATA_FAILED error code added

# 2.0.274

* All assemblies are strong named

# 2.0.273

* Project.AccessLevel property added to local storage (schema version changed to v46)

# 2.0.272

* Comments.RelatedTo() returns a query object instead of a simple enumerable
* Project members are not fetched automatically any more when creating a local cache for the remote project. This allows to create cache for locked projects and also speeds up the caching when listing projects.
* IReadonlyRepository renamed to IReadOnlyRepository

# 2.0.271

* Client and Data
	* License object modified to reflect latest API changes:
		* Removed collaborators limits and usage
		* Added upgrade link (schema version changed to v45)
* Client Search API
	* Added multi-project search support
	* Fixed date range based search
	
# 2.0.270

* Client
	* Online client can create projects under a specific license
* Data
	* Local project can be set with a license identifier
	* Catalog - a component to store data not attached to a specific project (currently - licenses)
* Sync
	* Support for pulling licenses to local data component

# 2.0.269

* Fine grained view manipulation API wrappers added

# 2.0.268

* Search API wrapper added

# 2.0.267

* Fix a bug in local storage when linking clashes

# 2.0.266

* Added wrapper for query user profile endpoint. 
* Added missed user properties: *Thumbnail*, *Title*, *SkypeId*, *LinkedinId *on both *Client *and *Storage *levels
* Added *LastAccessedOn* and *Company* entity on Client level
* Db schema version v44

# 2.0.265

* Fix for db migration tracing (migration message was logged even if no migration is performed). 
* Pagination API for projects is simplified (incompatible API change)
* Added possibility to query projects from specific pods (*podFilter*)
* Added possibility to receive list of projects from pods immediately after response is arrived without waiting for slower pods
* Added pagination support for all entity listings in Client component
* Style of pagination API for BIM objects aligned with all other pagination APIs

# 2.0.264

* Fix database locking on opening.

# 2.0.263

* License query API wrapper added. 
* *ErrorCode.LicenseCheckFailed* added.

# 2.0.262

* Bugfix: exceptions on comments pull because of multithread db access on Android.

# 2.0.261

* Bugfix: storage.Clear(All) fails if there are local only todos with attachments in the storage.
* Bugfix: allow to push file attachments even if the file entity is not cached locally
* Db schema version v43

# 2.0.260

* Terminology change "*Issue*" -> “*Todo*” on all layers (both Client and Data components): 
    * interface name for the controller (*ITodosController*), 
    * entity class name (*Todo*), 
    * accessor for the todos controller (*IProjectClient.Todos*), 
    * literal and constants names (*EntityType.Todo*), 

# 2.0.259

* No changes in components, example app migrated to Trimble.Identity 1.0.80

# 2.0.258

* BIM Object Search API wrapper added to Client component

# 2.0.256

* Fixes for automatic migration (v41->v42):
    * Migration is atomic and transactional now so opening the old db simultaneously from several threads/processes is not a problem. If migration fails the databases is guarantees in the original state
    * Backup file with original state is created along with the converted database
    * Trace events are written when conversion is performed

# 2.0.255

* Tracing added on synchronization errors in addition to error callback
* Project thumbnail push implemented
* MobileExample is updated with more functionality and with network stats

# 2.0.254

* *IPullableRepository* and *IPushableRepository* interfaces extracted
* *IPullableRepository* is implemented by *IEntityRepository* and *IPrincipalRepository*. 
* *IComments* interface fixed so it does not implement *IPullableRepository* (similar to *IClashes*)

# 2.0.253

* Fixes for handling recursive delete scenarios in the File Local storage
* Fixed synchronization for File Local storage when there are many local changes (create, move/rename, delete, content change). E.g. previously it was possible that the file moved out of the folder that is deleted locally get deleted instead of moved.
* Local Storage data schema changed to v42. Automating data migration implemented for v41 storage schema.
* New exception type is introduced: MigrationNotFoundException. Throws if automatic migration script is not found.

# 2.0.252

* Package metadata updated: icon changed to TC logo

# 2.0.251

* Package metadata updated: project url, license url, license acceptance is enforced
* HT API without base64 encoding is taken in use to simplify debugging

# 2.0.250

* Fix: Files.RefreshAsync() robustness improved in case server rejects some changes done locally

# 2.0.249

* Improvements to reduce locking when pulling Views, fixes database locked exception when pulling embedded Views

# 2.0.248

* Fixes bug when pull of a view (which has been deleted remotely or is otherwise unavailable) by identifier would result in null reference exception

# 2.0.247

* Resolves issue when embedded views would not be synced 

# 2.0.246

* No changes. Test added to CI.

# 2.0.244

* Namespace fixed for Trimble.Connect.Data.Models.AlignmentExtensions.

# 2.0.243

* Alignment structure is replaced with Placement in Local Storage.
* IStorage.Alignments collection is renamed to IStorage.Placements
* Other correspondent renamings
* Compatibility tools added to convert Alignment to Placement and back: Trimble.Connect.Data.Models.AlignmentExtensions.ToAlignment() and ToPlacement().

# 2.0.242

* Fix: File alignment brakes on sync when using scale and rotation

# 2.0.241

* License text (TRIMBLE CONNECT SDK LICENSE AGREEMENT Version 1.0) added to distribution packages.

# 2.0.240

* Project thumbnail is pulled to local storage as all other thumbnails.
* More robust download for view and attachments thumbnails (download retried next time if failed)

# 2.0.239

* No changes. 2.0.237 and 2.0.238 precompiled packages were not published properly because of teamcity upgrade.

# 2.0.238

* Fix: Storage.Clear method should remove thumbnails as well. New option "*ClearOptions.NotReferencedThumbnails*" added.

# 2.0.237

* Attachments API has changed to support soft link concept. THis way we support attachments that user has no access to. Now attachments are implemented as a soft link that belong to the parent entity (Issue). Attachment targets might be missed in the local storage until explicitly pulled.
* Batch linking and unlinking API to handle big number of attachments
* Pull for attachments is combined with pull for issues and included into the heartbeat logic.
* Push for attachments combined with push for issues.
* PROJECT_DELETED, USER_NOT_IN_PROJECT error codes added
* Support referencing to specific file versions as attachments from issues
* View and attachments thumbnail pull is included into heartbeat logic.
* ModifiedAttachments entity state added
* Get local id by global identifier and type API fixed
* Support undo scenario for removing synchronized link

# 2.0.234

* Implement ArrowMarkup API change - remove End1 and End2 properties in the model for online wrappers, first position is implicitly understood as arrow end. 

# 2.0.233

* Fix: sync cursors are reset when clearing the storage removes entities which might exist remotely

# 2.0.232

* Optimize pull for clashes. Interface changed a little bit (callback for each clash item removed) to take into account that there could be massive amount of clash items.

# 2.0.231

* Fix: view camera calculation

# 2.0.230

* Fix: clash set status is not updated on pull
* Fix: clash items sync in case sync started when clash is still executing
* Fix: sync error callback is not called for clashes and attachments

# 2.0.229

* Fix: Exception when error logging when pulling attachments

# 2.0.228

* Heartbeat based pull support. Provides significant performance improvement when pulling data from server.
* Fix: Cloud markup position translation

# 2.0.227

* Sync error recovery framework
* detailing reporting sync errors
* Reset operation to reset entity state and discard local changes
* Fix: Handle scenario when view is created for missed local file
* Fix: Handle permission revocation scenario
* Fix: Work with modified deleted entities correctly: delete state wins

# 2.0.226

* Support for clash items as attachments to Issues
* Fix: More robust alignment receive. Continue on error when processing alignments so other alignments are received
* Fix: More robust comments receive. Continue on a single error so other comments are received.
* Clash data model fixed (Clearance property)

# 2.0.225

* Embedded View API changes
* Support for atomic view update with thumbnail

# 2.0.220

* Alignment API changes
* Embedded View API changes

# 2.0.219

* Support for the alignment delete/reset operation in local storage and sync.
* Fix: Handle remotely deleted entities properly when pulling specific entities.

# 2.0.218

* Fix "database malformed" error in iOS when pulling views.

# 2.0.217

* Fix for thumbnail push on embedded view creation (thumbnail missed locally after push).

# 2.0.216

* Fix for thumbnail push on embedded view creation (thumbnail is not pushed).

# 2.0.215

* Enforce thumbnail download on pulling specific view. This fixes the pull of thumbnail for embedded view.
* Thumbnail push with embedded view fixed
* Added method to embed view with thumbnail

# 2.0.214

* Support referencing to file version from clashSet
* Support referencing to file version from View
* clashset endpoint change
* Fix: Use warnings level for tracing, not WriteLine
* Fix exception handling when pushing entities
* Fix "undelete" logic when regaining access to the entity

#  2.0.211

* Project.LastVisitedOn logic is changed to take into account that this field is not initialized on creation.
* Matrix based Alignment API recovered 
* Alignment delete API is implemented as reset operation

# 2.0.208

* Copyright texts updated
* Alignment endpoint change
* Alignment matrix API removed on backend. Emulating this on the Client layer until further clarifications.
* Clashsets endpoint change
* Clashes controller renamed to Clashsets

# 2.0.204

* Entity.Scope property is exposed to easily recognize embedded attachments
* ReleaseStatus.Received is removed from API.
* Fix: correct file references in view filters on file push.
* Embedded views support on all layers: client, local storage sync.
* Attachment.PushAsync() behavior changed so that local only attachments are pushed on demand automatically

# 2.0.188

Sync:

* fix for view sync bug "Pushing view breaks camera direction if direction is along x or y axis"

# 2.0.187

Client:

* new internal APIs, not exposed to clients
* removed obsolete Grids property from Views

Data:

* Removed obsolete VisibleGrids property from Views

# 2.0.186

Data:

* ClearOption.AttachedProperties added. Storage.Clear(All) also removes all attached properties now.
* Fix: storage database stays locked after version mismatch exception

# 2.0.185

Packages descriptors updated. New names for components without internal acronyms.

Data:

* enumeration for clashes collection fixed

# 2.0.183

Packages descriptors corrected with author information, documentation and license urls.

Client, Data, Sync:

* obsolete view presentation properties removed: PartColors, VisibleParts, SelectedParts

# 2.0.182

Data:

* fix: entity identifiers are unique for each entity type, not globally

Sync:

* fix: cannot push locally created model alignment
* User status property synchronization

# 2.0.178

Client, Data, Sync:

* New Clash item data model.
* Search clashes by model element identifier
* ModelElement concept introduced

# 2.0.177

Client, Data, Sync:

* View.Elements and ElementTypes support added.

# 2.0.176

Data and Sync:

* Tolerate single errors on refreshing model alignments, continue with next alignment after error
* Tolerate single errors on pulling comments, continue with next comment after error

# 2.0.174

Client:

* ClashSet.Files property is added

# 2.0.171

Client:

* View.Highlight property is removed
* View.Files property is added

Data and sync:

* Clear(options) method added
* Highlight property is removed
* Project statistics added

# 2.0.169

Data and Sync:

* support for clashsets and clashes in local storage with sync

# 2.0.167

Data and Sync:

* support for offline file operations like rename, move and delete and sync
* comments for files and folders offline and sync

# 2.0.166

Client:

* internal heartbeat API added

# 2.0.165

Data

* Files can be attached to issues and synced as attachments
* EntityRepository.Delete() will wipe deleted entity from db
* parent folder id is not exposed any mode. Use LocalFile.ParentFolder
* refactor IFileQuery interface. Move Get(id) methods to IFiles.
* NotDeleted file query added
* Root folder is not exposed as project property. Use IFiles.RootFolder.
* Project.Save() renamed to Update()

# 2.0.164

Sync

* Fix: view sync camera position now gives same view both in TCW and TCD regardless of where view is created

# 2.0.163

Data

* use local ids for ViewGroup->Views references

# 2.0.161

Data

* Use local ids in Attachments API
* Use local ids in Comments API
* Obsolete Save method removed

# 2.0.160

Sync

* Fix: single entity push not working
* Fix: if locally deleted entity is updated later, then it will be restored on push

# 2.0.159

Sync

* Atomicity for SyncClient.CreateStorageAsync() method improved: if something goes wrong this method guarantees that there is no half initialized local storage in the file system.

# 2.0.158

Client

* UploadFromFileAsync and DownloadToFileAsync methods
* Fix for Project.LastVisited time conversion

Sync

* PullAsync() behaviour changed when pulling collection: if one of the entities is failed all others are still pulled. Previously pull stopped on first error and throwed exception.
* PushAsync() behaviour changed when pushing collection: if one of the entities is failed all others are still pushed. Previously push stopped on first error and throwed exception.
* When receiving view (pull) if markup translation fails it is ignored. I.e. all other information for view is pulled, but markups.

# 2.0.157

Client

* New response error code added: USER_NOT_FOUND

# 2.0.155

Client

* clash item data schema changed in the TC API
* Get folder info by folder version added
* GetLatestInfoAsync() and GetVersionInfoAsync() extension methods added to work with both files and folders

Data

* LocalFile.MoveTo() operation added
* LocalFile.Undelete() operation added

# 2.0.154

Nuget package icons changed

Client

* measure with pick markup data schema changed in the TC API
* Atomic view creation with thumbnail supported now

Data

* createdBy and modifiedBy for alignment is supported now

# 2.0.152

Client

* Person.Status property added
* workaround for a backend issue where the id property is missing for invited and removed project members
* removed workaround: other than 404 error codes when checking for deleted objects

# 2.0.147

Data

* Alignment uses file local id for referencing
* internal refactoring to use local id for referencing to make data more robust when pushing data and global ids are changing

# 2.0.146

Data

* added API to Groups repository to support efficient querying of user groups for a user

# 2.0.145

Data

* API to access local storage is refactored to be more robust and error prone
* Save method is divided to Insert and Update to make intention explicit
* All modification methods use local Id as an object key instead of global identifier.
* local data access interface is separated from synchronizable repository interface
* interfaces for entity collections and principals collections are unified
* terminology change "collection" -> “repository”

# 2.0.144

Data

* added support for assigning users/group to views

Sync

* sync support for views assignees
* sync support for scenarios when user is assigned, unassigned, reassigned from a view

# 2.0.143

Client

* added method to create view with thumbnail in a single request

Sync

* On view push use a method to create view with thumbnail in a single request

# 2.0.139

Data

* EntityState added
* Entity metadata (creation and modification timestamps and users) is enforced on modification operations

# 2.0.138

Data

* Relations collection is renamed to Attachments
* EntityDescriptor concept introduced

Sync

* added API to pull individual entities
* Issue Attachments synchronization

# 2.0.137

Data

* User groups in local storage
* User groups can be assigned to Issues.

# 2.0.136

Sync

* Improved local db access parallelism: push operations don’t lock database for long period any more

Data

* Views.Save overload with thumbnail is transactional now.

# 2.0.135

Sync

* fix the view redlines markup translation error
* fix db constraint fail on thumbnail update after push
* fix: download view thumbnails on pull each time view has been changed on server
* fixed PullAsync() callback logic so the callback is called only for updated entities

# 2.0.134

Client

* Clash API

# 2.0.133

# Data:

* Plane representation in measurement pick has changed to keep information about picked point on the plane

# 2.0.132

# Data:

* File query by identifier API is improved (breaking change).

# 2.0.131

# Sync:

* bugfix for exceptions when synchronizing cloud storage modified by user other than creator
* bugfix for wrong current user in storage synced from the cloud

# 2.0.130

Client

* Folder permissions support

# 2.0.129

Client

* View.Assignees property added

# 2.0.128

Sync

* Fix: push folder hierarchy

# 2.0.127

Sync

* Syncing comments for views
* Comments sync API improved to provide more options to the application what to sync. Now it is possible to sync comments for specific entity only.

# 2.0.126

Data 

* IEntityCollection<T>.Get(long) method added to get entity by local id
* View.Models property is automatically updated on File push
* ViewGroup.Views property is automatically updated on View push

# 2.0.125

Data 

* Support Issue-View relation

* Support comment Issues and Views

# 2.0.124

Client 

* The url for the TC ViewGroups API has been changed

# 2.0.122

Sync 

* Performance improvement: don’t override members descriptors on pulling other entities if they exists already

# 2.0.121

Data 

* Bugfix: cannot add read only file to local storage

# 2.0.120

Client 

* Login with API key added

# 2.0.119

Data 

* LastModifiedOn supported on all collections in local storage and on a storage level

# 2.0.118

Client

* GetProjectsAsync() and GetProjectsPagedAsync() return partial results even if secondary pods are down.

# 2.0.117

Data

* view thumbnail sync

# 2.0.116

Client

* base url property to include the "/2.0/" as a last segment now

# 2.0.115-alpha

Client

* Add user groups API wrappers

# 2.0.114-alpha

Data

* Fix: Discard() local only file cause incorrect file status set

# 2.0.113-alpha

Data

* ViewGroup behavior aligned with backend: view can be member of any number of groups, view is not deleted when group is deleted. Relation is not enforced on database level any more.

# 2.0.112-alpha

Sync

* former projects members that has been removed from the project are marked in local database with Role=null.

# 2.0.111-alpha

Client

* string properties are used instead of enums in DTO to improve forward compatibility

Data

* IssuePriority and IssueStatus enums introduced

# 2.0.110-alpha

.NET 4.0 target platform is now supported for all components

# 2.0.109-alpha

Data

* ViewGroup.OrderOfChildren -> ViewGroup.Views renamed

Sync

* view group synchronization

# 2.0.108-alpha

Client:

* full view support with markups

Data

* view and markup data model aligned with backend. Many not backward compatible changes in the local data schema.

Sync

* view synchronization implemented (except thumbnails). A little bit experimental code for translation of camera, bit arrays and markups

# 2.0.105

Client:

* Release API wrappers.

# 2.0.104

Client:

* Improvement: More robust calculation for the file download progress based on availability of the Content-Length header.

# 2.0.102

Client:

* Fix: connection leak on GetProjectsAsync()

# 2.0.101-alpha

Sync:

* Fix: pushing locally created project is not possible

# 2.0.98-alpha

Sync:

* Pull optimized to minimise disk IO

# 2.0.97-alpha

Client:

* Support for other than file attachments for todos
* Extensibility point: dictionary of "unknown" properties for all entities

# 2.0.96-alpha

Client:

* FileTransferTimeout property exposed in interface

# 2.0.95-alpha

Client:

* Snapshot API alignment: endpoint changed, snapshot renamed to View
* Camera view angle and scale added

Data

* ProjectionType enum added

# 2.0.94

Sync:

* Improvement: Pushing locally created project is more robust (don’t depend on fixed error codes from server).

# 2.0.93-alpha

Sync:

* Fix: pulling files with identical context confuse file state.

# 2.0.92-alpha

Data:

* Improvement: More robust entity state handling (missing row in db interpreted as not synced entity).

# 2.0.91-alpha

Data:

* Improvement: transactional initialization.

# 2.0.90-alpha

Data:

* Improvement: TrygetProperty.

# 2.0.89

Data:

* Fix: entity deletion when the instance is constructed by the app.

# 2.0.88-alpha

Client:

* Improvement: FileTransferTimeout introduced.

# 2.0.86

No changes, version changed to publish a stable (not pre-release) package.

# 2.0.85-alpha

# Client:

* renamed ShardingConfiguration -> Configuration

Data:

* renamed CurrentUser -> CurrentUserIdentifier
* Automatically generate entity.Identifier if it is not initialized on Save()
* Automatically generate project.RootFolderIdentifier if it is not initialized 

Sync:

* New: locally offline created project can be pushed (see [documentation](https://docs.google.com/document/d/1RpqAPfrKxAfgssbbEiDMrRv_Utp-dQqjea3sFkBYr9w/edit#heading=h.unmlzty5rjqd))
* IsModified property added
* IsLocalOnly property added

# 2.0.82-alpha

Data:

* Improvement: DebuggerDisplay attribute added to model classes

# 2.0.80-alpha

Sync:

* Improvement: Performance optimizations for pull. Comments pull dramatically improved by using parallelization

# 2.0.77-alpha

Sync:

* Fix: Delete-delete conflict handling.

# 2.0.76-alpha

Sync:

* Issues and Comments sync optimized.

# 2.0.75-alpha

All dlls are build in release mode now

Data:

* View -> Snapshot, ViewGroup -> SnapshotGroup
* Storage.Exists method added

# 2.0.73-alpha

Symbol packages published

# 2.0.72-alpha

No changes.

# 2.0.70-alpha

All dlls are signed with Tekla key.

Client

* Fix: error response parsing to the InvalidServiceOperationException in case of empty body
* Workaround for difference between TC API specification and implementation: file/folder images support
* Fix: consistent naming for ThumbnailUrl property for project, File and Snapshot

# 2.0.60-alpha

Sync

* Fix: local only project settings (unit formatting and presentation) are lost on push/pull

# 2.0.59-alpha

Sync

* Improvement: File Sync API is taken in use
* Fix: When pushing a file don’t create a new version if file is not modified
* Fix: Don’t allow to pull file if there are local changes, require conflict resolution first

# 2.0.56-alpha

Data

* View.Preview is back

# 2.0.55-alpha

Sync

* New: model alignment sync

# 2.0.54-alpha

Data

* improvement: internal id exposed for all entities
* new SQLite.NET wrapper version taken in use (better error messages, thread safe)
* Project.Preview removed

# 2.0.53-alpha

Sync

* improvement: project descriptor sync with local changes detection
* new: last visited timestamp sync
* fix: sync of todos deletion with comments

# 2.0.52-alpha

Sync

* bugfix: wrong file status for added file (regression after 2.0.42-alpha)

# 2.0.49-alpha

Client

* TC v2 API alignment wrappers

# 2.0.48-alpha

Sync

* bugfix: add-add conflict handling for files and folders

# 2.0.44-alpha

Sync

* bugfix: removing issues with comments

# 2.0.42-alpha

Sync

* bugfix: wrong file status for deleted file

# 2.0.41-alpha

Sync

* File sync API improvements: concurrency check on push, Rebase and Discard methods added

Client

* Concurrency check on file upload
* File upload API cleanup
* Private file sync API added

# 2.0.40-alpha

Sync

* project members, issues, comments synchronization

# 2.0.37-alpha

Data

* Modelnfo alignment model reverted back to previously used

# 2.0.32-alpha

Client

- Alignment API changed so matrix is used both in input and output

# 2.0.30-alpha

Data and Sync

* file and folder delete remotely scenario supported
* file and folder name client side validation added
* robustness for file content transfer operation improved

# 2.0.29-alpha

Data

* not supported properties from ModelInfo removed

# 2.0.28-alpha

Data

* foreign key constraint enforcement fixed (cannot push locally created folders)

# 2.0.26-alpha

Data and Sync

* native folders support in local storage
* File hierarchy synchronization implemented including file/folder rename and move

# 2.0.24-alpha

Client

* InvalidServiceOperationexception.FromResponse made public

# Release 2.0

- TID authentication
- v2 APIs and TCD data schema alignment

# Release 1.0

This release is ment to be a replacement for the GTeam .NET Client component created for Dimentions 2014 integrations.
From now on the GTeam .NET Client component is deprocated and these SDK components should bused instead for all new development.

- Authentication context component works with GTeam authentication mechanism
- REST API wrappers. All Currently available GTeam APIs are covered.
- web browser pupup for sign-in
- token cache
