# Trimble Connect .NET SDK Release Notes

(Applicable for Trimble.Connect.Data and Trimble.Connect.Data.Sync components)

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
