# Trimble Connect Client .NET Release Notes

# 2.8.1
* Removed Xamarin.iOS10 from target frameworks.
* Added new GET and UPDATE methods for folder permission APIs. Removed the older permission methods. This will be a breaking change on the folder permission APIs.

# 2.7.10
* Added Bulk API for fetching heartbeats of multiple projects for sync applications.

# 2.7.9
* Added progress invocation changes in Multipart DownloadAsync method.
* Added Multipart Download API IFilesController.DownloadAsync() which returns part files for advanced streaming scenarios.

# 2.7.8
* Made internals visible to SyncHelper Client.

# 2.7.7
* Added Slope Measurement markup type.
* Proceed reading markups from the response even if some of the markups are not supported by the client

# 2.7.6
* Made internals visible to model client.
* Added support for File Permission API.

# 2.7.5
* Made internals visible to sync mirror.

# 2.7.4
* Fixed IncludeAttachment query param in IFilesController.GetSnapshot() method.
* Updated Get Projects method to limit page size to 25 when specific license parameters are included in the request.

# 2.7.3
* Added UploadAsync method to IFilesController to support uploading files from public URLs.

# 2.7.2
* Added the TidUuid property explicitly to Person. This can be a breaking change when you are already accessing this property using the properties dictionary. 

# 2.7.1
* Added Multipart Download API IFilesController.DownloadAsync()
* Fixed the assembly version issue.

# 2.6.70
* Added locks to shared resources during multipart upload.

# 2.6.69
* Added logs to troubleshoot large file uploads.
* Increased part size for large files (> 5 GB)

# 2.6.68
* Fixed threading issue in multipart upload url refresh when upload url expired during parallel uploads.
* Removed net40 support.

# 2.6.67
* Added check sum validation for part uploads.

# 2.6.66
* Added query param support to IFilesController.GetSnapshot
* Added ModifiedBy field to the response of FilesController.UploadAsync

# 2.6.65
* Added query param support to IFilesController.DownloadAsync

# 2.6.64
* Updated TrimbleConnectClient.RequestAccessAsync(projectId) API to newer Projects/RequestProjectAccess API that will send notification email to project admins. 
* Updated minimum target android version to monoandroid71 (API level 25)
* Removed IProjectClient.Models and IModelsController that had retired BIM Apis.

# 2.6.63
* Added support for revision field as part of the PackageUploadResponse class. 

# 2.6.62
* Added query param support to IProjectMembersController.GetAllAsync() API.

# 2.6.61
* Updated the IUsersController.GetProfileAsync() to return only the current user information. 

# 2.6.60
* Skip passing authentication token in ITrimbleConnectClient.DownloadThumbnailAsync() by default.
* Updated the `Selenium.WebDriver.ChromeDriver` module to the latest version (118.0.5993.7000).

# 2.6.59
* Added query param support to IProjectClient.GetAsync API. Clients can use fullyLoaded = false query param to get minimal project details.
* Added IProjectClient.Location property to get the project location.

# 2.6.58
* Implemented internal routing of all upload calls in FilesController to PackageUpload.

# 2.6.57
* Added support for account information in License class.

# 2.6.56
* Updated the `Trimble.Identity.OAuth.Password` module to the latest version (1.0.25).

# 2.6.55
* Handle the package upload errors during initiate phase.

# 2.6.54
* Fixed the progress issue.

# 2.6.53
* Fixed updating the url to use in multipart upload on retry after refreshing the part urls.

# 2.6.52
* Added Wait flag to improve the upload status polling in IFilesController.GetPackageUploadInfo.
* Updated the `Selenium.WebDriver.ChromeDriver` module to the latest version (116.0.5845.9600).
* Added Sync session Identifier param in IFilesController.UploadPackageAsync.

# 2.6.51
* Added support for parallel part uploads.

# 2.6.50
* Added support for refreshing multipart upload urls on expiry.

# 2.6.49
* Added support for multipart package upload in IFilesController.UploadPackageAsync API.

# 2.6.48
* Fixed last visited timestamp to be culture independent.
* Updated read me file and documentation links.

# 2.6.47
* Added topic case to the DeltaResponseConverter to add topicId to the View properties.

# 2.6.46
* Updated the `Trimble.Connect.Client.Common` module to the latest version (1.0.82).

# 2.6.45
* Removed preprocessor directive present in FileControllerExtensions for DownloadToFile methods.

# 2.6.44
* Fixed task cancelled exception in successive async calls on Trimble Connect Client by closing the response stream on previous calls.

# 2.6.43
* Updated position values in Angle markup to be nullable.

# 2.6.42
* Added fileset parameter to multipart file upload.

# 2.6.41
* Added minimal flag support in ProjectMembersController.GetAsync() to fetch either partial or full response.

# 2.6.40
* Added IFilesController.GetSnapshot() Api to get the latest snapshot of files and folders in the project.

# 2.6.39
# 2.6.38
* Updated the `Trimble.Diagnostics` module to the latest version (3.0.3).
* Updated the `Trimble.Connect.Client.Common` module to the latest version (1.0.79).

# 2.6.37
* Added PanPositionX and PanPositionY properties in View2D class.

# 2.6.36
* Updated `Newtonsoft.Json` module to 13.0.1.
* Updated the `Trimble.Connect.Client.Common` module to the latest version (1.0.78).
* Updated the `Selenium.WebDriver.ChromeDriver` module to the latest version (105.0.5195.5200).

# 2.6.35
* Updated the `Selenium.WebDriver.ChromeDriver` module to the latest version (103.0.5060.5300).
* Updated the `Trimble.Diagnostics` module to the latest version (3.0.3) for netstandard 2.0 target framework.
* Updated the `Trimble.Connect.Client.Common` module to the latest version (1.0.77).

# 2.6.34
* Added Position properties in Angle markup class.

# 2.6.33
* Removed the null value handling for Position properties in MeasurementPick.

# 2.6.32
* Added support for angle markup.
* Updated the `Selenium.WebDriver.ChromeDriver` module to the latest version (102.0.5005.6102).

# 2.6.31
* Updated the `Selenium.WebDriver.ChromeDriver` module to the latest version (100.0.4896.6000).

# 2.6.30
* Added support to pass processing header to the package upload API IFilesController.UploadPackageAsync().

# 2.6.29
* Added support for `Topics` or any generic object type in `Tags` entity.

# 2.6.28
* Added support for package upload API.
* Removed unused file formats (f3d, xml, property and propertyset) and deprecated bsq format.

# 2.6.27
* Use test automation accounts in test cases.
* Updated the `Trimble.Connect.Client.Common` module to the latest version (1.0.72).

# 2.6.26
* Fix: Issue in view json parser. The camera has to be parsed as null for large views.

# 2.6.25
* File version identifier can be passed to Get, Set and Delete Alignment calls.

# 2.6.24
* Added name, description and stock keeping unit properties to License entity.

# 2.6.23
* Added notify parameter to IProjectMembersController.AddAsync() API.

# 2.6.22
* Updated the `Selenium.WebDriver.ChromeDriver` module to the latest version (94.0.4606.4101).

# 2.6.21
* Updated the `Trimble.Identity.OAuth.Password` module to the latest version (1.0.20).
* Added `Selenium.WebDriver.ChromeDriver` module to the Trimble.Connect.Client.Test.

# 2.6.20
* Updated the `Trimble.Identity.OAuth.Password` module to the latest version (1.0.18).
* Updated the `Trimble.Connect.Client.Common` module to the latest version (1.0.65).
* Updated the `Newtonsoft.Json` module to (12.0.3) version for netstandard2.0 Targetframework to fix the whitesource vulnerability.
* Updated the `NUnit` module to latest version (3.13.2) to fix the whitesource vulnerability.

# 2.6.19
* Updated the `Trimble.Identity.OAuth.Password` module to the latest version (1.0.17).
* Updated the `Trimble.Connect.Client.Common` module to the latest version (1.0.62).

# 2.6.18
* Updated the `Trimble.Identity.OAuth.Password` module to the latest version (1.0.15).
* Removed RetryHandler class from `Trimble.Connect.Client`and the retry is handled by default in IClientConfig.RetryConfig if the TrimbleConnectClient is initialized using ICredentialsProvider.

# 2.6.17
* Implemented the latest representation fileUpload API workflow.

# 2.6.16
* Updated the `Trimble.Identity.OAuth.Password` module to the latest version (1.0.13).
* Updated the `Trimble.Connect.Client.Common` module to the latest version (1.0.57).

# 2.6.15
* Updated the `Trimble.Identity.OAuth.Password` module to the latest version (1.0.12).
* Updated the `Trimble.Connect.Client.Common` module to the latest version (1.0.56).

# 2.6.14
* Added support for crs and address fields to project meta data.

# 2.6.13
* Fix: Issue with todo label not set.

# 2.6.12
* Updated the TC logo image url for the nuget package

# 2.6.11
* Fix: Skip passing authorization header for file download using pre-signed url.
* Fix: Issue with paging in fetching activities.

# 2.6.10-beta
* Fix: Skip invalid markups while parsing views response.

# 2.6.9-beta
* Renamed IProjectClient.Share as IProjectClient.Shares for better maintainability.

# 2.6.8-beta
* Removed ISharesController and replaced it with IController<Shares>
* Implemented Activities endpoint.

# 2.6.7-beta
* Removed ISharesController.GetAllShares() and replaced it with ISharesController.GetSharesPagedAsync()

# 2.6.6-beta
* Made polling interval configurable for IFilesController.AssimilationCompletedAsync() and IClashSetsController.CompletedAsync()
* Added ITrimbleConnectClient.InitializeTrimbleConnectUserAsync() to initialize the Trimble connect user.

# 2.6.5-beta
* Added: Shares API support.

# 2.6.4-beta
* Integrated 1.0.51-beta version of trimble.Connect.Client.Common

# 2.6.3-beta
* Integrated 1.0.49-beta version of Trimble.Connect.Client.Common by off-loading the regions implementation to common client.

# 2.6.2-beta
* Added CredentialsProvider to ITrimbleConnectClient to support other micro service client intializations like sync helper client using IProjectClient

# 2.6.1-beta
* Added new constructor to initialize Trimble Connect Client using ICredentialProvider and marked the other constructors obsolete.
* Deprecated Netstandard1.4 target and uap target (Netstandard2.0 target can be used for UWP development).
* Generating NuGet symbols package in the new (snupkg) format.

# 2.6.0
* Integrated with 1.0.43 version of Trimble.Connect.Client.Common component.
* Added a constructor for TrimbleConnectClient taking TrimbleConnectClientConfig parameter.
* The Timeout property in ITrimbleConnectClient is removed and it can be set using TrimbleConnectClientConfig.
* Retry handler is added by default.

# 2.5.5
* Added white source config file and fixed sonarqube bugs.

# 2.5.4
* Added an overload method for ITrimbleConnectClient.GetProjectsAsync() taking fullyLoaded parameter.
* Made StyleCop reference private.

# 2.5.3
* Reverted to use old regions endpoint in ITrimbleConnectClient.ReadConfigurationAsync()

# 2.5.2
* Modified GetRegionsAsync() to hit the new static region end point. 
* Adapted the Region response to include the region specific end points for other microservices like pset, org.

# 2.5.1
* Adapted the Trimble.Connect.Client to use Trimble.Connect.Client.Common component

# 2.5.0
* Added netstandard 2.0 target.
* Removed Trimble.Connect.PSet.Client dependency that was used to get and set quick access items. 
* P-Set operations are not supported in this package and clients should make use of Trimble.Connect.PSet.Client nuget package directly.

# 2.4.23
* Fix: Downloading 2d files as pdf file format.

# 2.4.22
* Fix: Added SourceId property to ModelElementTree

# 2.4.21
* Fix: Added If-Match header in file upload request to ensure file updation against latest version.
* Deprecated old multipart upload API.

# 2.4.20
* Added fileset and pathtemplate parameters to Upload and Download APIs to support point cloud files.

# 2.4.19
* Fix: Errors in view json parsor with respect to parsing the assignees.

# 2.4.18
* Added panPositionX and panPositionY parameters to View2D while creating new View2D.

# 2.4.17
* Added support for skipLargeViewDetail query param while pulling views so that large view details are not pulled.
* The views will be flagged as large if the view metadata is greater than 500KB.
* Fix: Errors in view json parsor by adding cases to parse additional view properties.

# 2.4.16
* Added overload that allows passing download url to IFilesController.DownloadAsync method to support multipart download.

# 2.4.15
# 2.4.14
* Fix: Out of memory exception on parsing large view response object.

# 2.4.13
* Integrated generic pset client for quick access items.

# 2.4.12
* Fixed query parameters support for GetProjectsAsync API.

# 2.4.11
# 2.4.10
* Fixed sort order in quick access items.

# 2.4.9
# 2.4.8
* Added API support to get and set quick access items for Trimble Connect User. This API gets/sets the items from/to Trimble CDM p-set service.

# 2.4.7
* Use only access token from TID to initialize Trimble connect User. 

# 2.4.6
* Added API support to get the user information within a project.

# 2.4.5
* Added pdf fileformat to Trimble.Connect.Client.Models.FileFormat enumeration.

# 2.4.4
* Changed the authentication for Trimble Connect APIs to use TID access token
* Remarks: ITrimbleConnectClient.LoginAsync that exchanges TID token for connect access token is obsolete now. Instead clients should use ITrimbleConnectClient.InitializeTrimbleConnectUserAsync.

# 2.4.3
* Added API support to request the application access token
* Added support for downloading 2D files as pdf file format.

# 2.4.2
# 2.4.1
* Integrated the data ocean file services APIs.
* Added API support for links with embedded file target.

# 2.3.5
# 2.3.4
* Added paging support to get members of company.

# 2.3.3
* Added Timezone property to Person entity type.
* Added API support to get the timezones supported by Trimble Connect.

# 2.3.2
* Added cache handler delegate support to Trimble connect http client that currently caches heart beat responses.

# 2.3.1
* Modified percentage complete field in the delta response to double precision.

# 2.3.0
* Added support for FsObject stream (EntityType.FsObject) that combines file and folder in a signle size optimized stream.
* Added IsDeleted flag in Entities to get deleted status, the deletedOn and deletedBy are transferred in Entity.ModifiedOn and Entity.ModifiedBy properties.
* Added ability to set the ITrimbleConnectClient.Configuration property from the app. This allow to save one network call if configuration is known to the app already. 

# 2.2.17
* Added overload that allows to pass query parameters to IFilesController.UpdateFileAsync.

# 2.2.16
* Added overload that allows to pass query parameters to file listing APIs: IFilesController.GetFileVersionsAsync, IFilesController.GetFolderItemsAsync, IFilesController.GetFolderItemsByPathAsync.

# 2.2.15
* Pagination fixed for the THUMBNAIL stream in ObjectSync API

# 2.2.14
* Added method overload for providing additional parameters during the file upload. API: UploadAsync

# 2.2.13
* Added paging capability for all file and folder APIs: IFilesController.GetFolderVersionsAsync, IFilesController.GetFolderItemsByPathAsync, IFilesController.GetFolderItemsAsync, IFilesController.GetFileVersionsAsync.
* Fixed digital signature missing in 2.2.12

# 2.2.12
* ViewGroup entity type name (`EntityType.ViewGroup`) fixed: VIEWGROUP ->  VIEW_GROUP. Allows to use object sync API for view group.

# 2.2.10
* Improved the extensibility of the `ITrimbleConnectClient.CreateProjectAsync()` and `IProjectClient.UpdateAsync()` operations. 
  Now project properties that are not part of the `Project` DTO definition can be updated by adding them to generic `Project.Properties` dictionary.
* `Project.GroupsCount` property added

# 2.2.9
* Add missing Heartbeat support for thumbnails cursor

# 2.2.8
* Add support for optimized thumbnail and triangle count sync

# 2.2.7
* License text updated: https://community.trimble.com/docs/DOC-10021  
* Upgrade dependencies so they point to packages with correct license

# 2.2.6
* Fix: protect from duplicate calls to `ITrimbleConnectClient.AddUserAgent()` to add version of the same component multiple times
* Added `FileFormat.TrimBIM` constant

# 2.2.5
* Make custom UsrAgent headers tolerant to possible errors. If header cannot be added by any reason the client instance still can be constracted.

# 2.2.4
* `ITrimbleConnectClient.AddUserAgent()` method added to allow adding custom entries to the UserAgent header sent with all requests

# 2.2.3
* Added ability to add arbitrary parameters to ObjectSync API queries

# 2.2.2
* Fix authenticode signing

# 2.2.1
* downgrade Android target framework 7.0 -> 4.4

# 2.2.0
* FileTransferTimeout is removed
* netstandard1.4 target introduced. Profile111 is removed

# 2.1.42
* Fix: when paging listing results (projects, entities, BIM objects) one last item might be lost

# 2.1.41
* Remove id from the set alignment request body

# 2.1.40
* `ReceiveAll()` extension method renamed to `ReceiveAllAsync()`
* Reduce the default page size for ObjectSync API to 50

# 2.1.39
* Add UploadRepresentationAsync to IFilesController to support uploading alternative representations of model files

# 2.1.38
* Add support for reading and updating project's unit settings and invite and todo restrictions

# 2.1.37
* Added support for url attachment names

# 2.1.36
* Added: IAppsController interface: 
   - wrapper to query app version information with download url
   - wrapper to query app info
   - wrapper to request API key and app access token
* The App settings wrappers are reimplemented to accommodate changes in the TCPS API.
* App tokens API added: 'IAppsController.CurrentApp' property added to handle requests that require app level authentication (e.g. 'IAppsController.SetSettings' and 'IAppsController.GetSettings').
* Fix: file download ('IFileController.DownloadAsync') operation uses wrong http timeout (same as normal object requests)

# 2.1.35
* The TransferUtility.DownloadAsync() internal algorithm is improved (file is not downloaded in-place, but to temporary file first with locking)

# 2.1.34
* Added object sync support for entity links

# 2.1.33
* Added support for entity links

# 2.1.32
* Improvement: Expose setters for Attachment.VersionId and Attachment.Url properties.

# 2.1.31
* Fix: app settings cannot be set for the very first time.
* Added: support attachments for comments.
* added: support for URL attachments (for todos and comments). 'EntityType.Url' and 'UrlReference' are added.

# 2.1.30
* Improvement: The application settings API wrapper has changed (incompatible change comparing to 2.1.29) to support concurrency. The 'AppSettings' data model is introduced.

# 2.1.29
* Added: Application settings (per user cross project) API wrapper ('IUsersController.GetAppSettingsAsync' and 'IUsersController.SetAppSettingsAsync').
Implementation is based on preliminary backend API and subject to change, the intention is to provide an interface for integrators to start working with.

# 2.1.28
* Improvement: Increase the default page size when requesting changes to 100
* Fix: one last item can be lost from the Object Sync API response in rare cases.

# 2.1.27
* Improvement: 'InvalidServiceOperationException.Data' property is populated with all properties from the error message sent by server
* 'LoginOptions' and 'LoginAsync(string, LoginOptions)' are extracted to a separate Trimble.Connect.Auth component to decouple the WebUI dependency

# 2.1.26
* Added: Support uploading embedded attachments with multipart upload and 'TransferUtility'.
* API change: TransferUtility.NumberOfThreads -> TransferUtility.MaxParallelSegments
* Improvement: default timeout for 'TransferUtility' set to 2 minutes (was infinite).

# 2.1.25
* Added: App can control the level of parallelization when using 'TransferUtility'.
* Improved: 'TransferUtility' automatically switches to simple upload mode if file is small.
* Added: 'TransferUtility.DownloadAsync' method added to perform resumable file download.

# 2.1.24
* Incompatible API change in the 'IFilesController.RemoveFolderPermissionsAsync' method to accept collection of permissions to remove.

# 2.1.23
* Added: Multipart file upload low level API wrapper
* Added: 'TransferUtility' with multipart file upload high level API

# 2.1.22
* User profile object expanded with new properties: 'Language', 'Companies', 'Phone', 'DefaultPodLocation', 'ViewerBackgroundColor'.
* Company profile object expanded with new properties: 'CreatedBy', 'ModifiedBy', 'ModifiedOn', 'Role'.
* Added 'IUserController.UpdateAsync(Person)'
* Added 'ICompaniesController'
* 'IViews.GetAllAsync' does not add a 'detail=true' query parameter by default any more

# 2.1.21
* triangles count property added for Models

# 2.1.20
* Improvement: The *IReleasesController* interface is aligned with *ITodosController* and *ITagsController* so return type is ObjectIdentity, not Attachment. 

# 2.1.19
* Improvement: The *ITodosController* interface is changed (incompatible change) to return collection of Entities instead of collection of Attachments. 
* Improvement: Most of additional properties are removed from the *Attachment* class to keep it as an entity descriptor only.
* Improvement: *EntityDescritor* class is removed as duplicate. *Attachment* class to be used instead everywhere.
* Improvement: *ITagsController* interface has changed to use *Attachment* class as entity descriptor instead of *EntityDescritor*.
* Added: *Entity.ToDescriptor()* and *Entity.IsEmbedded()* extensions are exposed.

# 2.1.18
* Fix: Cannot get object sync response beyond first page

# 2.1.17
* Added: Support embedded file attachments
* Added: FileInfo.ParentType property

# 2.1.16
* Bugfix for Tags Object Sync API support

# 2.1.15
* Added: support Object Sync API for Tags

# 2.1.14
* Added: support Object Sync API for 2D views

# 2.1.13
* Added: request list of supported file formats for assimilation API
* Fix: fixed name for RequestAccessAsync method (was RequestAccess)

# 2.1.12
* Added: request access to the locked project API
* Fix: fixed name for EntityType.View2D (was EntityType.View2)

# 2.1.11
* Added: 2d views API support

# 2.1.10
* Added: tags API support

# 2.1.9
* Add support for model hierarchy, properties and property sets download

# 2.1.8
* Fix an issue where calling UploadFromFileAsync with name==null wouldn't extract name from file path but use null instead
* Internal improvements of automated tests

# 2.1.7
* Rename extensions: UploadWithTimeoutAsync -> UploadAsync, DownloadToFileWithTimeoutAsync -> DownloadToFileAsync.
* Add possibility to specify transfer buffer size for DownloadToFileAsync (default buffer size is 80KB).
* Add possibility to specify transfer buffer size for file upload operations.

# 2.1.6
* Extensions are added (UploadWithTimeoutAsync, DownloadToFileWithTimeoutAsync) to allow to upload and download file content with timeout on a stream activity.

# 2.1.5

* Predefined Todo types are removed

# 2.1.4

* Added: Todo.CompletionPercentage property

# 2.1.3

* Fix: first version of the file is added to release always.

# 2.1.2

* Added: pagination support for fetching clash items.

# 2.1.1

* Minor version incremented because binary compartibility was broken in 2.0.289 (version should have been incremented back then)
* Todo.Title and Type properties are added to data model
* FolderItem.Status, HasChildren and AccessLevel properties are added to data model

# 2.0.292

* Added FolderItem.Revision property

# 2.0.291

* Missed extension added for deleting attachments from Todo
* The attachments API assymetry is encapsulated: now the result from listing attachments can be directly used for deleting attachments. 
Background: TCPS API uses attachment id property differently in requests and in responses. In request it is a file *version id*, in response it is a *file id* and the *file version id* is tranferred in a different field. This creates incompartibility between response and request content.
The solution applied on SDK level is that `Attachent.Identifier` and `Attachent.Version` properties are always used according to their names, but when sending requests to TCPS the `Identifier` is automatically replaced with value from the `Version` property when needed.

# 2.0.290

* `IProjectClient` interface now inherits from `ITrimbleConnectClient` interface, so all cross-project methods can be invoked on a ProjectClient as well

# 2.0.289

* Cleanup `IController` `ITrimbleConnectClient` interfaces, move overloads to extension methods

# 2.0.288

* Fix the breaking API change in the 2.0.287: `IController.GetAllAsync()` overload is missing
* Fix the breaking API change in the 2.0.287: `ITrimbleConnectClient.SearchAsync()` overload is missing
* Added: extensibility mechanism to send additional parameters in query string in `ITrimbleConnectClient.GetProjectsAsync()`

# 2.0.287

* Added: extensibility mechanism to send additional parameters in query string in `GetAllAsync()`
* Added: extensibility mechanism `InvokeApiAsync()`
* Added: convenience extensions to search `Views` by model and model version
* Added: convenience extensions to search `Todos` by file and file version 

# 2.0.286

* `ReceiveAll()` extension method added to enumerate paged results
* internal refactorings to improve support for object delta queries
* improve robustness when parsing range for paged results

## Common Release notes
(Applicable for Trimble.Connect.Client, Trimble.Connect.Data, Trimble.Connect.Data.Sync components)

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
