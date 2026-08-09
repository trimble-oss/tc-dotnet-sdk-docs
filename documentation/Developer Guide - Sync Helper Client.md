
# Trimble.Connect.SyncHelperService.Client .NET SDK Developer Guide

## Content

1. [Overview](#overview)
2. [Authentication](#authentication)
3. [Get File And Folder Structure](#get-file-and-folder-structure)
4. [Get Permission HearBeat And Events ](#get-permission-heartbeat-and-events)
	
	


### Overview

The Object sync helper service client component helps to fetch the file and folder structures, as well as the permission heartbeat and permission events of the project.

### Authentication


TC uses token based authentication. The token is acquired during the TID OAuth2 (OpenID Connect) authentication (see  [Trimble.ID Developer Guide](https://github.com/trimble-oss/trimble-id-sdk-docs-for-net)).

It is important to understand that the ID token is generally a one use token with short life span. This means that when the token is expired and a new TID token needs to be acquired. App will have to request a new id token from TID. This can be done by  
`AuthenticationContext.AcquireTokenByRefreshTokenAsync`  or by using  `RefreshOptions.IdToken`  option in other  `AuthenticationContext.AcquireTokenAsync`  methods. Please refer to the  [Trimble.ID Developer Guide](https://github.com/trimble-oss/trimble-id-sdk-docs-for-net).

The clients can use Trimble.Identity.OAuth.AuthCode for interactive workflow authentication or Trimble.Identity.OAuth.Password for headless applications (like from test code). These newer components provide higher levels of abstraction and encapsulate the token acquiring and refreshing part. The clients can also provide any custom implementation of ICredentialsProvider.
### Example: Authenticating and Initializing.

Authenticate with TID initialize and SyncHelperClient. 

  ```
var clientCreds = new ClientCredential(ClientId, ClientKey)
        {
            RedirectUri = new Uri(RedirectUrl),
        };
var authCtx = new AuthenticationContext(clientCreds)
        {
            AuthorityUri = new Uri(AuthorityUrl),
        };
var provider = new AuthCodeCredentialsProvider(authCtx)
        {
            AuthenticationRequest = new InteractiveAuthenticationRequest();
        };
var config = new TrimbleConnectClientConfig { ServiceURI = "https://objsync-api.connect.trimble.com/"};
config.RetryConfig = new RetryConfig { MaxErrorRetry = 1 };
using (var syncHelperClient = new SyncHelperClient(config, provider))
{    
	await syncHelperClient.ReadRegionsAsync().ConfigureAwait(false);
	...
}
```
															
### Get File And Folder Structure
#### Project File Structure
The GetProjectFileStructure method returns the latest file structure of the project as an array of FolderItems. Project Identifier is the mandatory to be passed to fetch the project items.

	var folderItems = syncHelperClient .GetProjectFileStructure(project.Identifier, cancellationToken)
	

 - This method returns only the latest version of the folder/files. 
 - The readonly for the latest version will be returned.
 - The deleted versions will not be returned.
 - The ParentIdentifier will only have the latest Parent Version Identifier.

#### Folder Structure
The GetFolderStructure method gets the content of the folder including all the subfolders and files. This method returns an array of FolderItem entities. Project identifier and folder identifier are mandatory fields to be passed.

	var folderStructure = syncHelperClient .GetFolderStructure(project.Identifiter, folderId, cancellationToken);

														
### Get Permission HeartBeat And Events

#### Permission HeartBeat
The GetPermissionHeartBeat returns information about permission heartbeat from the server, of type HeartBeat. 

	var heartbeat = await syncHelperClient.GetPermissionHeartbeatAsync(project.Identifier, cancellationToken);

The heartbeat contains the latest permissionCursor. When the permission is not set, status code (404) **Not Found** will be returned.

#### Permission Events
The GetPermissionEventsAsync method fetches the permission events from the server and returns the response of type FolderPermissionResponse.

		bool hasMore;     
           var request= await syncHelperClient.GetPermissionEventsAsync(this.Client.ProjectIdentifier, permissionCursor, pageSize, cancellationToken: cancellationToken);

           do
           {
               var response = await request.ConfigureAwait(false);
               permissionCursor = response.Cursor;
               
               hasMore = !string.IsNullOrEmpty(response.Cursor);

               if (hasMore)
               {
                   request = await syncHelperClient.GetPermissionEventsAsync(this.Client.ProjectIdentifier, permissionCursor, pageSize, cancellationToken: cancellationToken);
               }  
		 }while(hasMore);

PageSize is an optional parameter which can be passed, null can be passed if pagenation is controlled by the server. 
	
The FolderPermissionResponse contains FolderPermission array which is a collection of folder permissions which have been modified.

