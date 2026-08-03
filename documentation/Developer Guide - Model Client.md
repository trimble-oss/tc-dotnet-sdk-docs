# Trimble.Connect.Model.Client .NET SDK Developer Guide

### Content

1. [Introduction](#introduction)
2. [Who should use this guide](#audience)
3. [Components and dependencies](#components)
4. [NuGet package](#nuget-package)
5. [Environments and endpoints](#environments)
6. [Authentication](#authentication)
9. [Paging](#paging)
10. [HTTP handlers, retries, and errors](#http-and-errors)
11. [API usage](#api-usage)
    - [Model info](#model-info)
    - [Entities](#entities)
    - [Entity aggregation](#entity-aggregation)
    - [Batch get](#batch-get)
    - [Property set definitions](#property-set-definitions)
    - [Hierarchy, history, layers, owners](#other-listings)
    - [NWD submodels and download URL](#nwd-support)

### Acronyms

| Acronym | Meaning |
|---------|---------|
| TID | Trimble Identity |
| TC | Trimble Connect |
| NWD | Navisworks file format processed by the Model service |
| TRB | Trimble rendering binary produced for NWD submodels |

### Terminology

| Concept | Description |
|---------|-------------|
| Model ID | Trimble Connect file identifier for an uploaded, processed model |
| Version ID | File version identifier; use when querying a specific model version |
| Entity | A building element or object inside a processed model |
| Property set (pset) | Metadata attached to entities, described by property set definitions |
| Submodel | A child model inside a processed NWD file |
| Submodel CURIE | A compound identifier (`model:...#...:version:...`) used for submodel-specific download URLs |

---

## <a name="introduction">Introduction</a>

**Trimble.Connect.Model.Client** is a .NET client library for the Trimble Connect **Model REST API**. It provides strongly typed request/response models and async methods for reading processed model data: entities, hierarchies, layers, property sets, history, batch operations, and NWD submodel download URLs.

The client is designed for:

- **Integrators** building desktop, web, or mobile .NET applications that read Trimble Connect model data.
- **Product teams** inside Trimble consuming the published NuGet package.
- **SDK maintainers** contributing to this repository.

The Model Client is part of the broader Trimble Connect .NET SDK ecosystem. It depends on **Trimble.Connect.Client** for HTTP infrastructure, authentication token handling, and (optionally) file upload/assimilation through `IFilesController`. For general TC platform concepts, authentication patterns, and project management, see the [TC .NET SDK Developer Guide](https://github.com/trimble-oss/tc-dotnet-sdk-docs/tree/main/documentation).

**API specification:** https://app.swaggerhub.com/apis/Trimble-Connect/model/v1

### Why use the Model Client instead of raw REST?

- **Strong typing** — compile-time checks and IntelliSense for requests and responses.
- **Built-in infrastructure** — HTTP formatting, error handling, token management, and retry configuration via shared TC client components.
- **Pagination helpers** — `ReceiveAllByOffsetAsync` and `ReceiveAllBySkipTokenAsync` extension methods for listing APIs.
- **Cross-platform** — targets .NET Framework 4.5+ and .NET Standard 2.0 (Xamarin iOS/Android supported via MSBuild.Sdk.Extras).

---

## <a name="audience">Who should use this guide</a>

| Audience | Start here |
|----------|------------|
| **Integrator / product consumer** | Sections 4–13: install the NuGet package, authenticate, configure endpoints, and call APIs. |
| **Repository maintainer** | Section 14: build, test, pack, and release from this repo. |

---

## <a name="components">Components and dependencies</a>

This repository contains a single packable library and its integration tests:

| Project | Purpose |
|---------|---------|
| `Trimble.Connect.Model.Client` | Public NuGet package (`Trimble.Connect.Model.Client`) |
| `Trimble.Connect.Model.Client.Test` | Integration tests against live STAGE services |

Key public types:

| Type | Role |
|------|------|
| `ModelServiceClient` | Main client implementation |
| `IModelServiceClient` | Client interface |
| `ModelServiceClientConfig` | Endpoint, environment, region, retry, and HTTP handler configuration |
| `ModelServiceClientRequestExtensions` | Pagination extension methods |
| `Models.Requests.*` | Request DTOs (one per API operation) |
| `Models.*` | Response DTOs |

NuGet dependencies (transitive packages are pulled in automatically):

- `Trimble.Connect.Client` — HTTP client, `ICredentialsProvider`, `IFilesController`, `RegionsConfig`
- `Trimble.Connect.Client.Common` — shared utilities
- `Newtonsoft.Json` — JSON serialization

For authentication you also need a **Trimble.Identity** credentials provider (not a direct dependency of the Model Client package, but required at runtime). Common choices:

- `Trimble.Identity.OAuth.AuthCode` — interactive browser login
- `Trimble.Identity.OAuth.Password` — resource-owner password (testing/automation only)

---

## <a name="nuget-package">NuGet package</a>

**Package ID:** `Trimble.Connect.Model.Client`

**Supported target frameworks:**

- .NET Framework 4.5+
- .NET Standard 2.0

Install from NuGet:

```powershell
dotnet add package Trimble.Connect.Model.Client
```

Official releases are published to the Trimble Connect NuGet feed. See [Support](#support) for package source and credential requests.

Symbol packages (`.snupkg`) are produced alongside the library for debugging.

---

## <a name="environments">Environments and endpoints</a>

The Model service is deployed per region. Configure the client using one of the approaches below.

### Option 1: Explicit `ServiceURI` (recommended for getting started)

Set the base URI directly on `ModelServiceClientConfig`:

```csharp
var config = new ModelServiceClientConfig
{
    ServiceURI = new Uri("https://model-api11.stage.connect.trimble.com/"),
};
```

Use the regional Model API host that matches your Trimble Connect project location.

### Option 2: `Environment` + `Region`

`ModelServiceClientConfig` maps environment and region codes to service hosts:

| Environment | Region | Host |
|-------------|--------|------|
| STAGE | NA | `https://model-api11.stage.connect.trimble.com` |
| STAGE | EU | `https://model-api21.stage.connect.trimble.com` |
| STAGE | AP | `https://model-api31.stage.connect.trimble.com` |
| PROD | NA | `https://model-api11.connect.trimble.com` |
| PROD | EU | `https://model-api21.connect.trimble.com` |
| PROD | UK | `https://model-api22.connect.trimble.com` |
| PROD | AP | `https://model-api31.connect.trimble.com` |
| PROD | AU | `https://model-api32.connect.trimble.com` |

```csharp
var config = new ModelServiceClientConfig
{
    Environment = "STAGE",
    Region = "NA",
};
```

### Option 3: Project location via `RegionsConfig`

When `Region` is a Trimble Connect project location (for example `northAmerica`), initialize region metadata from the TC API first:

```csharp
using Trimble.Connect.Client.Common;

RegionsConfig.RegionsUri = new Uri("https://app.stage.connect.trimble.com/tc/api/2.0/regions");
await RegionsConfig.InitializeFromServerAsync(credentialsProvider).ConfigureAwait(false);

var config = new ModelServiceClientConfig
{
    Environment = "STAGE",
    Region = project.Location, // e.g. "northAmerica"
};
```

If `RegionsConfig` is not initialized, endpoint resolution throws `InvalidOperationException`.

### Trimble Identity environments

| Environment | Authority URL |
|-------------|---------------|
| Staging | `https://stage.id.trimblecloud.com/oauth/` |
| Production | `https://id.trimble.com/oauth/` |

Staging TID is used with STAGE TC services; production TID with PROD services.

---

## <a name="authentication">Authentication</a>

All Model API calls require a valid Trimble Identity access token. Pass any `ICredentialsProvider` implementation to `ModelServiceClient`.

### Interactive auth (desktop / web)

```csharp
using System;
using Trimble.Connect.Model.Client;
using Trimble.Connect.Model.Client.Implementation;
using Trimble.Identity;
using Trimble.Identity.OAuth.AuthCode;

var authCtx = new AuthContext(clientKey, clientName, redirectUrl)
{
    AuthorityUri = new Uri("https://stage.id.trimblecloud.com/oauth/"),
};

var credentialsProvider = new AuthCodeCredentialsProvider(authCtx);

var config = new ModelServiceClientConfig
{
    ServiceURI = new Uri("https://model-api11.stage.connect.trimble.com/"),
};

using var modelClient = new ModelServiceClient(config, credentialsProvider);
```
### Reusing credentials from Trimble Connect Client

If your app already uses `TrimbleConnectClient`, reuse its `CredentialsProvider`:

```csharp
using var tcClient = new TrimbleConnectClient(tcConfig, credentialsProvider);
await tcClient.ReadConfigurationAsync().ConfigureAwait(false);
await tcClient.InitializeTrimbleConnectUserAsync().ConfigureAwait(false);

var projectClient = await tcClient.GetProjectClientAsync(project).ConfigureAwait(false);

using var modelClient = new ModelServiceClient(
    new ModelServiceClientConfig { ServiceURI = modelServiceUri },
    tcClient.CredentialsProvider,
    projectClient.Files);
```

Passing `projectClient.Files` enables model upload and assimilation helpers on `modelClient.Files`.

---

## <a name="init-model-client">Initialize the client</a>

```csharp
using Trimble.Connect.Model.Client;
using Trimble.Connect.Model.Client.Implementation;

var config = new ModelServiceClientConfig
{
    Environment = "STAGE",
    Region = "NA",
};

// Optional: configure retries (inherited from ClientConfig)
config.RetryConfig = new RetryConfig { MaxErrorRetry = 3 };

using var modelClient = new ModelServiceClient(config, credentialsProvider);
```
---

```csharp
using System.Diagnostics;
using System.IO;

// modelClient.Files is non-null when IFilesController was passed to the constructor
var file = await modelClient.Files.UploadFromFileAsync(
    project.RootFolderIdentifier,
    localFilePath,
    "column.ifc").ConfigureAwait(false);

var sw = Stopwatch.StartNew();
var status = await modelClient.Files.AssimilationCompletedAsync(
    file.Identifier,
    progress: percent => Console.WriteLine("Processing: {0}% ({1} ms)", percent, sw.ElapsedMilliseconds))
    .ConfigureAwait(false);

// status == 100 means processing completed successfully
string modelId = file.Identifier;
string versionId = file.VersionIdentifier;
```

Use `modelId` and `versionId` in subsequent Model API calls (`ListEntitiesAsync`, `GetModelAsync`, etc.).

---

## <a name="paging">Paging</a>

Listing APIs use one of two pagination models.

### Offset pagination (`top` / `offset`)

Used by entities, hierarchies, history, layers, owners, property sets, and identifiers. Request pages manually:

```csharp
var request = new ListEntitiesRequest
{
    ModelId = modelId,
    Top = 100,
    Offset = 0,
};

var page = await modelClient.ListEntitiesAsync(request).ConfigureAwait(false);
// Use page.OffsetNext to fetch the next page
```

Or collect all pages with the extension method:

```csharp
using Trimble.Connect.Model.Client;
using Trimble.Connect.Model.Client.Models.Entities;
using Trimble.Connect.Model.Client.Models.Requests.Entities;

var request = new ListEntitiesRequest
{
    ModelId = modelId,
    Top = 100,
    Offset = 0,
};

await modelClient.ReceiveAllByOffsetAsync<Entities>(request, page =>
{
    foreach (var entity in page.Items)
    {
        // process entity
    }
}).ConfigureAwait(false);
```

`ReceiveAllByOffsetAsync` only works with GET requests that return `IOffsetPagedResponse`.

### Cursor pagination (`pageSize` / `skipToken`)

Used by NWD submodel listing:

```csharp
using Trimble.Connect.Model.Client.Models.Requests.Submodels;
using Trimble.Connect.Model.Client.Models.Submodels;

var request = new ListSubmodels
{
    ModelId = nwdModelId,
    PageSize = 50,
};

await modelClient.ReceiveAllBySkipTokenAsync<SubmodelItems>(request, page =>
{
    foreach (var submodel in page.Items)
    {
        // process submodel
    }
}).ConfigureAwait(false);
```

---

## <a name="http-and-errors">HTTP handlers, retries, and errors</a>

### Delegating handlers

Add custom `DelegatingHandler` instances to inspect or modify HTTP traffic (logging, correlation IDs, etc.):

```csharp
var config = new ModelServiceClientConfig
{
    ServiceURI = serviceUri,
};
config.HttpHandlers.Add(myHandler);

using var client = new ModelServiceClient(config, credentialsProvider);
```

### Retries

Configure retry behavior through `RetryConfig` on `ModelServiceClientConfig` (inherited from `ClientConfig`):

```csharp
config.RetryConfig = new RetryConfig { MaxErrorRetry = 3 };
```

### Exceptions

| Exception | When |
|-----------|------|
| `ArgumentNullException` | Required request property is null (validated before the HTTP call) |
| `ArgumentException` | Required string property is empty |
| `InvalidDataException` | Invalid request data or unresolvable service URI |
| `InvalidOperationException` | Client-side failure (e.g. pagination helper used with a non-GET request) |
| `InvalidServiceOperationException` | Model API returned an error response |
| `OperationCanceledException` | Request was cancelled via `CancellationToken` |

All public async methods accept an optional `CancellationToken`.

### Low-level requests

For operations not yet wrapped by a typed method, use `SendAsync<TResponse>`:

```csharp
var response = await modelClient.SendAsync<MyCustomResponse>(myRequest, cancellationToken)
    .ConfigureAwait(false);
```

---

## <a name="api-usage">API usage</a>

The sections below cover the main API areas. Each request class lives under `Trimble.Connect.Model.Client.Models.Requests` and links to the Swagger operation in its XML documentation.

### <a name="model-info">Model info</a>

```csharp
using Trimble.Connect.Model.Client.Models.Requests.Model;

var request = new GetModelRequest
{
    ModelId = modelId,
    VersionId = versionId,
};

var model = await modelClient.GetModelAsync(request).ConfigureAwait(false);
```

### <a name="entities">Entities</a>

```csharp
using Trimble.Connect.Model.Client.Models.Requests.Entities;

// List entities
var listRequest = new ListEntitiesRequest
{
    ModelId = modelId,
    VersionId = versionId,
    Top = 50,
    Fields = new List<string> { "id", "type", "psets", "boundingBox" },
};

var entities = await modelClient.ListEntitiesAsync(listRequest).ConfigureAwait(false);

// Get entity by ID
var getRequest = new GetEntityIdRequest
{
    ModelId = modelId,
    EntityId = entityId,
};

var entity = await modelClient.GetEntityIdAsync(getRequest).ConfigureAwait(false);

// List source identifiers
var idRequest = new ListIdentifiersRequest { ModelId = modelId };
var identifiers = await modelClient.ListIdentifiersAsync(idRequest).ConfigureAwait(false);
```

**`Fields` parameter:** Controls which properties are returned. Supported values include `id`, `idx`, `idc`, `type`, `psets`, `psets.name`, `boundingBox`, `product`, `hierarchyInfo`, `layerIds`. When `fields` is specified, the `include` parameter is ignored.

**`$filter`:** OData filter expression. When set, `text`, `hierarchyType`, and `type` parameters are ignored.

### <a name="entity-aggregation">Entity aggregation</a>

```csharp
using Trimble.Connect.Model.Client.Models.Requests.Entities;

// Count all entities
var countRequest = new GetAggregateEntitiesRequest
{
    ModelId = modelId,
    Apply = "aggregate($count as Count)",
};
var countResult = await modelClient.AggregateEntitiesAsync(countRequest).ConfigureAwait(false);

// Group by type
var groupRequest = new GetAggregateEntitiesRequest
{
    ModelId = modelId,
    Apply = "groupby((type), aggregate($count as Count))",
    OrderBy = "Count desc",
};
var grouped = await modelClient.AggregateEntitiesAsync(groupRequest).ConfigureAwait(false);
```

### <a name="batch-get">Batch get</a>

Fetch multiple entities and property set definitions in a single request:

```csharp
using Trimble.Connect.Model.Client.Models.Requests.BatchOperations;

var request = new GetBatchRequest
{
    ModelId = modelId,
    VersionId = versionId,
    Fields = new List<string> { "id", "idx", "type", "psets" },
    Entities = new List<BatchGetEntityRequest>
    {
        new BatchGetEntityRequest { Idx = 1 },
    },
    PSetDefs = new List<BatchGetPSetDefRequest>
    {
        new BatchGetPSetDefRequest { Idx = 0 },
    },
};

var response = await modelClient.ListBatchOperationsAsync(request).ConfigureAwait(false);
```

### <a name="property-set-definitions">Property set definitions</a>

```csharp
using Trimble.Connect.Model.Client.Models.Requests.PSet;
using Trimble.Connect.Model.Client.Models.Requests.PSet.Index;

// List definitions
var listRequest = new ListPSetRequest { ModelId = modelId };
var psetList = await modelClient.ListPSetDefs(listRequest).ConfigureAwait(false);

// Get definition by index
var getRequest = new GetPsetIndexRequest { ModelId = modelId, Idx = 0 };
var psetDef = await modelClient.GetPSetDef(getRequest).ConfigureAwait(false);
```

### <a name="other-listings">Hierarchy, history, layers, owners</a>

```csharp
using Trimble.Connect.Model.Client.Models.Requests.Hierarchy;
using Trimble.Connect.Model.Client.Models.Requests.History;
using Trimble.Connect.Model.Client.Models.Requests.Layer;
using Trimble.Connect.Model.Client.Models.Requests.Owners;

var hierarchy = await modelClient.ListHierarchyAsync(
    new ListHierarchyRequest { ModelId = modelId }).ConfigureAwait(false);

var hierarchyByType = await modelClient.ListHierarchyTypeAsync(
    new ListHierarchyTypeRequest { ModelId = modelId, HierarchyType = "IFCBUILDINGSTOREY" })
    .ConfigureAwait(false);

var history = await modelClient.ListHistoryAsync(
    new ListHistoryRequest { ModelId = modelId, VersionId = versionId }).ConfigureAwait(false);

var layers = await modelClient.ListLayersAsync(
    new ListLayerRequest { ModelId = modelId }).ConfigureAwait(false);

var owners = await modelClient.ListOwnersAsync(
    new ListOwnersRequest { ModelId = modelId }).ConfigureAwait(false);
```

### <a name="nwd-support">NWD submodels and download URL</a>

Submodel listing and TRB download URLs apply to **processed NWD models only**. Calling these APIs against non-NWD models (for example IFC) returns `InvalidServiceOperationException`.

```csharp
using Trimble.Connect.Model.Client.Models.Requests.Submodels;
using DownloadUrlRequest = Trimble.Connect.Model.Client.Models.Requests.DownloadUrl.GetDownloadUrl;

// List submodels
var submodels = await modelClient.ListSubmodelsAsync(new ListSubmodels
{
    ModelId = nwdModelId,
    VersionId = versionId,
    PageSize = 50,
    Fields = new List<string> { "trbSize", "boundingBox" },
}).ConfigureAwait(false);

// Presigned TRB download URL for a file ID
var download = await modelClient.GetDownloadUrlAsync(new DownloadUrlRequest
{
    ModelId = nwdModelId,
    VersionId = versionId,
}).ConfigureAwait(false);

string trbUrl = download.DownloadUrlId;

// Presigned TRB download URL for a submodel CURIE
var submodelDownload = await modelClient.GetDownloadUrlAsync(new DownloadUrlRequest
{
    ModelId = "model:S0WnnDmRpMs#0e1b3600f1097404:version:S0WnnDmRpMs",
}).ConfigureAwait(false);
```

---