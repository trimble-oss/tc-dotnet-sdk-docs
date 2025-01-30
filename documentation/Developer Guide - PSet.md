# Trimble.Connect.PSet.Client.NET SDK Developer Guide

### Content

1. [Overview](#overview)
2. [Authentication flow](#flow)
3. [Code tracing](#tracing)
4. [Code snippets](#snippets)
   + [Example: Initializes a new instance of the PSetClient Class](#init-pset-client)
   + [Example: How Paging is handled](#paging-example)
   + [Example: Delegating Handlers](#delegating-handlers)
   + [Example: CRUD Operations for Library](#crud-library)
   + [Example: CRUD Operations for Definitions](#crud-definitions)
   + [Example: CRUD Operations for PSet](#crud-pset)

### Acronyms

**TID** - Trimble Identity

## <a name="overview">Overview</a>

The Trimble Connect Property Set Service Client (PSet Client) provides .NET SDK wrappers for accessing and interacting with the Property Set Service in the Trimble Connect ecosystem. This service enables seamless management of property sets and their associated values across connected projects and assets.

### Key Features

- **Simplified Property Set Management**: Effortlessly create, update, and manage property sets and their attributes within Trimble Connect projects.
- **Integration with Trimble Connect**: Enables smooth interaction with other Trimble Connect APIs and services, ensuring a unified user experience.
- **Authentication Support**: Uses Trimble Identity (TID) for secure and scalable authentication.
- **Environment Options**:
  - STAGE: `https://pset-api.stage.connect.trimble.com/v1`
  - PROD: `https://pset-api.connect.trimble.com/v1`

### Core Concepts

- **Property Sets**: Logical groupings of properties or attributes associated with an object, such as a Trimble Connect project or file.
- **Attributes**: Individual data points within a property set, such as metadata or configuration details.
- **Service API Wrappers**: Simplifies integration with the underlying RESTful Property Set Service, allowing .NET developers to focus on their application's functionality.

## <a name="flow">Authentication flow</a>

The **PSetClient** follows an authentication flow that ensures secure access to Trimble Connect Services. The flow involves initializing the client with proper credentials and leveraging a credentials provider to obtain authentication tokens for API requests.

### Steps in the Authentication Flow

1. **Client Configuration Initialization**  
   A `PSetClientConfig` object is created, containing essential settings, including user-agent information for requests made to Trimble Connect Services.

2. **Credentials Provider Initialization**  
   The `ICredentialsProvider` is initialized to supply authentication tokens required for authenticated API requests. This provider manages token acquisition and refresh.

3. **Client Initialization**  
   The `PSetClient` constructor is called with the `PSetClientConfig` and `ICredentialsProvider` objects. Both the configuration and credentials provider are validated to ensure they are not null.

4. **Token Inclusion in Requests**  
   The `TrimbleConnectHttpClient` is created and linked with the `ICredentialsProvider`. This ensures that every request includes the necessary authentication token.

5. **Request Handling**  
   For each API call, the HTTP client retrieves a valid token from the credentials provider. If the token is invalid or expired, the provider refreshes it automatically before including it in the request headers.

### Code Example

Authenticate with TID, initialize `Trimble.Identity.OAuth.AuthCode`:

```csharp
var authCtx = new AuthContext(Config.ClientKey, Config.ClientName, Config.RedirectUrl)
{
    AuthorityUri = new Uri(AuthorityUrl),
};

var provider = new AuthCodeCredentialsProvider(authCtx)

var config = new PSetClientConfig { ServiceURI = TCServiceUri };

var psetClient = new PSetClient(config, provider);

```
instead of ServiceURI the user can also pass the region and environment as arguments.


### <a name="paging-example">Example of how paging is handled</a>

```csharp
var currentPage = await psetClient.ListDefinitionsAsync(request, cancellationToken).ConfigureAwait(false);

while (currentPage != null)
{
    cancellationToken.ThrowIfCancellationRequested();

    Task<DefinitionsPage> nextPageTask = null;

    if (currentPage.Next != null)
    {
        nextPageTask = psetClient.ListDefinitionsAsync(currentPage.Next, cancellationToken);
    }

    resultPageProcessor.Invoke(currentPage);
    currentPage = nextPageTask != null ? await nextPageTask.ConfigureAwait(false) : null;
}
```

### <a name="delegating-handlers">Example of Delegating Handlers</a>

```csharp
var handler = new Handler();
var config = new PSetClientConfig { ServiceURI = new Uri(Config.ServiceUrl) };
config.HttpHandlers.Add(handler);

using (var client = new PSetClient(config, Global.CredentialsProvider))
{
    await this.MakeAPICall(client).ConfigureAwait(false);
    Assert.AreEqual(1, handler.Called);
}
```

### <a name="crud-library">CRUD Operations for Library</a>

```csharp
// Get Library
var GetLibraryRequest = new GetLibraryRequest{
    LibraryId = $"LibraryId",
}

var getLibraryResponse = await PSetClient.GetLibraryAsync(GetLibraryRequest).ConfigureAwait(false);

// Create Library
var createLibraryRequest = new CreateLibraryRequest
{
    Id = $"WellKnownLibId",
    Name = $"WellKnownLibName",
    Description = libraryDescription,
};

var createLibraryResponse = await PSetClient.CreateLibraryAsync(createLibraryRequest).ConfigureAwait(false);

//Update Library
var UpdateLibraryRequest = new UpdateLibraryRequest
{
    LibraryId = $"WellKnownLibId",
    Name = $"WellKnownLibName",
    Description = libraryDescription,
    Version = versionid
};

var UpdateLibraryResponse = await PSetClient.UpdateLibraryAsync(UpdateLibraryRequest).ConfigureAwait(false);

//Delete Library
var DeleteLibraryRequest = new DeleteLibraryRequest
{
    LibraryId = $"WellKnownLibId",
};

var DeleteLibraryResponse = await PSetClient.DeleteLibraryAsync(DeleteLibraryRequest).ConfigureAwait(false);

```

### <a name="crud-definitions">CRUD Operations for Definitions</a>

```csharp
// Get Definition
var GetDefinitionRequest = new GetDefinitionAsync{
    LibraryId = $"WellKnownLibId",
    DefinitionId = $"DefinitionId"
}
var GetDefinitionResponse = await PSetClient.GetDefinitionAsync(GetDefinitionRequest).ConfigureAwait(false);

//List Definition
var ListDefinitionRequest = new ListDefinitionAsync{
    LibraryId = $"WellKnownLibId",
}
var ListDefinitionResponse = await PSetClient.ListDefinitionAsync(ListDefinitionRequest).ConfigureAwait(false);

// Create Definition 
var CreateDefinitionRequest = new CreateDefinitionAsync{
    LibraryId = $"WellKnownLibId",
    Version = Versionid,
    Type = $"ObjectTypes"
}
var CreateDefinitionResponse = await PSetClient.CreateDefinitionAsync(CreateDefinitionRequest).ConfigureAwait(false);

//Update Definition
var UpsertDefinitionRequest = new UpdateDefinitionAsync{
    LibraryId = $"WellKnownLibId",
    DefinitionId = $"DefinitionId",
    Type = $"ObjectTypes"
}
var CreateDefinitionResponse = await PSetClient.UpdateDefinitionAsync(UpsertDefinitionRequest).ConfigureAwait(false);

//Delete Definition
var DeleteDefinitionRequest = new DeleteDefinitionAsync{
    LibraryId = $"WellKnownLibId",
    DefinitionId = $"DefinitionId",
}
var DeleteDefinitionResponse = await PSetClient.DeleteDefinitionAsync(DeleteDefinitionRequest).ConfigureAwait(false);

```

### <a name="crud-pset">Fetching PSet</a>

```csharp
// Get Pset
var GetPSetRequest = new GetPSetAsync{
    LibraryId = $"WellKnownLibId",
    DefinitionId = $"DefinitionId",
    Version = versionid 
}
var GetPSetResponse = await PSetClient.GetPSetAsync(GetPSetRequest).ConfigureAwait(false);

//List Pset
var ListPSetsRequest = new ListPSetsAsync{
    LibraryId = $"WellKnownLibId",
    DefinitionId = $"DefinitionId"
}
var ListPSetResponse = await PSetClient.ListPSetsAsync(ListPSetsRequest).ConfigureAwait(false);

//Update Pset
var PutPSetRequest = new PutPSetAsync{
    LibraryId = $"WellKnownLibId",
    DefinitionId = $"DefinitionId"
}
var PutPSetResponse = await PSetClient.PutPSetAsync(PutPSetRequest).ConfigureAwait(false);

//Delete Pset
var DeletePSetRequest = new DeletePSetAsync{
    LibraryId = $"WellKnownLibId",
    DefinitionId = $"DefinitionId"
}
var DeletePSetResponse = await PSetClient.DeletePSetAsync(DeletePSetRequest).ConfigureAwait(false);

```

