# Trimble Connect .NET SDK Developer Guide — DPoP

### Content

1. [Overview](#overview)
2. [Can Project Sight use a DPoP token with Connect clients?](#can-project-sight-use-dpop)
3. [How DPoP is attached](#how-dpop-is-attached)
4. [Prerequisites](#prerequisites)
5. [Authentication / Initialization](#authentication)
   + [Create a `DpopCredentialProvider`](#create-provider)
   + [Initialize Trimble Connect Client](#init-connect)
   + [Initialize PSet, Topic, Model, Issue, and ECom clients](#init-other)
   + [Share one provider across clients](#share-provider)
   + [Regions configuration](#regions-config)
6. [Proof signing contract](#proof-signing)
7. [Retry, nonce, and token renewal](#retry-and-nonce)
8. [What Project Sight must not do](#must-not)
9. [File transfer and skip-authorization](#file-transfer)
10. [Code snippets](#snippets)
11. [Troubleshooting](#troubleshooting)
12. [Related guides](#related-guides)

### Acronyms

**TID** — Trimble Identity

**TC** — Trimble Connect

**DPoP** — Demonstrating Proof-of-Possession ([RFC 9449](https://datatracker.ietf.org/doc/html/rfc9449))

**RS** — Resource server (a Trimble Connect API)

## Overview

DPoP support is available from **`Trimble.Connect.Client.Common` 2.1.0**. Earlier Common versions (`2.0.x`) do not include `DpopCredentialProvider` or the retry-handler changes required for DPoP.

Trimble Connect .NET service clients authenticate through `ICredentialsProvider`. Historically that meant a Bearer access token from `Trimble.Identity.OAuth.AuthCode`, `Trimble.Identity.OAuth.Password`, or `TokenCredentialsProvider`.

Project Sight (and any other product that already holds a **DPoP-bound delegated access token**) can use the same clients. From 2.1.0 the common layer ships `DpopCredentialProvider`, an `ICredentialsProvider` that:

* Puts `Authorization: DPoP <access_token>` on each request (not `Bearer`).
* Puts a new `DPoP` proof JWT on each physical send.
* On HTTP 401, either renews the token or retries with a server nonce, depending on the challenge.
* On HTTP 5xx / 429 retries, signs a **new** proof so a previous `jti` is not replayed.

Token acquisition and proof signing stay in the calling application. The SDK does not talk to TID, does not hold the DPoP private key, and does not implement JWT signing.

The TC REST API specification can be found here:

* Staging: [https://app.stage.connect.trimble.com/tc/static/apidoc.html](https://app.stage.connect.trimble.com/tc/static/apidoc.html)
* Production: [https://app.connect.trimble.com/tc/static/apidoc.html](https://app.connect.trimble.com/tc/static/apidoc.html)

DPoP proof format and TID token-exchange details are documented by Trimble Identity:

* [Use DPoP authorization](https://docs.trimblecloud.com/trimble-identity/content/how-to-guides/integrate_signin/tid-use-dpop-authorization/index.html)
* [Obtain a DPoP proof](https://docs.trimblecloud.com/trimble-identity/content/how-to-guides/integrate_signin/obtain-dpop-proof/)

## Can Project Sight use a DPoP token with Connect clients?

**Yes.** Project Sight does not need a fork of `Trimble.Connect.Client`, `Trimble.Connect.PSet.Client`, or the other service wrappers.

Every current TC .NET service client already takes `ICredentialsProvider` and builds its HTTP pipeline with `TrimbleConnectHttpClient.Create(config, credentialsProvider)`. Passing `DpopCredentialProvider` is enough for the wrappers to send DPoP instead of Bearer.

| Client package | Constructor | Works with `DpopCredentialProvider` |
|---|---|---|
| `Trimble.Connect.Client` | `TrimbleConnectClient(TrimbleConnectClientConfig, ICredentialsProvider)` | Yes. `GetProjectClient` / `GetProjectClientAsync` reuse the same HTTP pipeline. |
| `Trimble.Connect.PSet.Client` | `PSetClient(PSetClientConfig, ICredentialsProvider)` | Yes. Requests are built without a Bearer header; the handler attaches DPoP. |
| `Trimble.Connect.Topic.Client` | `TopicServiceClient(TopicServiceClientConfig, ICredentialsProvider)` | Yes. Pass the same provider into `GetProjectClient`. |
| `Trimble.Connect.Model.Client` | `ModelServiceClient(ModelServiceClientConfig, ICredentialsProvider, IFilesController)` | Yes. Reuse the Connect client's provider (and optionally `projectClient.Files`). |
| `Trimble.Connect.Issue.Client` | `IssueServiceClient(IssueServiceClientConfig, ICredentialsProvider)` | Yes. |
| `Trimble.Connect.EComService.Client` | `TrimbleConnectEComServiceClient(EComServiceClientConfig, ICredentialsProvider)` | Yes. |
| Org client | `OrgClient(OrgClientConfig, ICredentialsProvider)` | Yes. |

Two conditions must both be true:

1. **Runtime Common package** — the process must load **`Trimble.Connect.Client.Common` 2.1.0 or later**, which contains `DpopCredentialProvider` **and** the retry-handler change that authorizes every physical send. Service client packages currently pin Common `2.0.1`–`2.0.5`. Add a **direct** `PackageReference` on Common `2.1.0` so NuGet does not keep the older handler.
2. **The target TC API must accept DPoP** — the SDK can only send the headers. Endpoints that still expect Bearer will return 401 even when the client is correct. Confirm the Project Sight endpoint list against the Connect DPoP rollout before switching a call path.

`AuthCodeCredentialsProvider` and `TokenCredentialsProvider` always set `Authorization: Bearer`. They cannot carry a DPoP-bound token.

## How DPoP is attached

```
Project Sight
  │  subject-delegated DPoP access token
  │  DPoP private key / proof signer
  ▼
DpopCredentialProvider  (ICredentialsProvider)
  │  getValidAccessTokenAsync
  │  renewAccessTokenAsync
  │  createProofAsync(method, uri, token, nonce)
  ▼
TrimbleConnectClient / PSetClient / TopicServiceClient / ...
  │  TrimbleConnectHttpClient.Create(config, provider)
  ▼
RetryHttpMessageHandler
  │  before each send: AuthorizeAsync
  │    Authorization: DPoP <token>
  │    DPoP: <proof JWT>
  │  on 401: InvalidateAndAuthorizeAsync, retry once
  │  on 5xx/429: new proof, retry per RetryConfig
  ▼
Trimble Connect APIs
```

When `TrimbleConnectClient` is constructed with a credentials provider, `GetUserAuthenticationToken()` returns `null`. Controllers still call `CreateRequest(..., GetUserAuthenticationToken())`, but `WithAuthBearer(null)` is a no-op. `RetryHttpMessageHandler` then attaches DPoP immediately before the request goes on the wire.

Newer clients (PSet, Topic, Model, Issue, ECom) never set Bearer themselves. They call `CreateRequest(method, uri)` and rely entirely on the provider.

## Prerequisites

1. A DPoP-bound access token for the Trimble Connect resource. For Project Sight this is the TID **subject-delegation** (token exchange) token, not a plain Bearer user or application token.
2. The same key pair used when that token was minted. `createProofAsync` must sign with the private key whose public JWK is bound to the token (`cnf.jkt`).
3. NuGet packages:
   * `Trimble.Connect.Client.Common` **2.1.0** or later.
   * The service client packages Project Sight already uses (`Trimble.Connect.Client`, `Trimble.Connect.PSet.Client`, and so on).
4. Direct `PackageReference` on Common **2.1.0** so it wins over the older Common version declared by the service clients:

```xml
<ItemGroup>
  <PackageReference Include="Trimble.Connect.Client" Version="2.8.11" />
  <PackageReference Include="Trimble.Connect.PSet.Client" Version="..." />
  <!-- Direct reference so the DPoP handler and DpopCredentialProvider are loaded. -->
  <PackageReference Include="Trimble.Connect.Client.Common" Version="2.1.0" />
</ItemGroup>
```

## Authentication / Initialization

### Create a `DpopCredentialProvider`

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;
using Trimble.Connect.Client;

ICredentialsProvider provider = new DpopCredentialProvider(
    getValidAccessTokenAsync: () => projectSightTokens.GetValidAccessTokenAsync(),
    renewAccessTokenAsync: () => projectSightTokens.RenewAccessTokenAsync(),
    createProofAsync: (method, uri, token, nonce) =>
        projectSightTokens.CreateProofAsync(method, uri, token, nonce));
```

| Delegate | When the SDK calls it | Project Sight responsibility |
|---|---|---|
| `getValidAccessTokenAsync` | Every authorize that is not a forced renewal. | Return a cached delegated token. Renew it **before expiry** if needed. Do not return null or empty. |
| `renewAccessTokenAsync` | HTTP 401 that is **not** a `use_dpop_nonce` challenge, or `InvalidateAndAuthorizeAsync(null, ...)`. | Discard the cached token, run token exchange again, return the new token. |
| `createProofAsync` | Every authorize, including nonce retries. Arguments: HTTP method, absolute request URI, access token, optional server nonce. | Return a compact DPoP proof JWT. Use a new `jti` every time. Include `nonce` in the payload when it is not null. |

The provider does not own token cache or key rotation. If Project Sight rotates the DPoP key when it renews the token, `renewAccessTokenAsync` and `createProofAsync` must stay coordinated so the next proof is signed with the key bound to the new token.

### Initialize Trimble Connect Client

This is the same shape as the [Client developer guide](https://github.com/trimble-oss/tc-dotnet-sdk-docs/blob/main/documentation/Developer%20Guide%20-%20Client.md), with `DpopCredentialProvider` in place of `AuthCodeCredentialsProvider`.

```csharp
using System;
using System.Linq;
using System.Threading.Tasks;
using Trimble.Connect.Client;
using Trimble.Connect.Client.Common;

var config = new TrimbleConnectClientConfig
{
    ServiceURI = new Uri("https://app.stage.connect.trimble.com/tc/api/2.0/"),
};
config.RetryConfig = new RetryConfig { MaxErrorRetry = 3 };

var provider = new DpopCredentialProvider(
    getValidAccessTokenAsync: GetValidDelegatedTokenAsync,
    renewAccessTokenAsync: RenewDelegatedTokenAsync,
    createProofAsync: CreateDpopProofAsync);

using (var client = new TrimbleConnectClient(config, provider))
{
    await client.InitializeTrimbleConnectUserAsync().ConfigureAwait(false);
    await client.ReadConfigurationAsync().ConfigureAwait(false);

    var projects = await client.GetProjectsAsync().ConfigureAwait(false);
    var project = projects.First();
    var projectClient = await client.GetProjectClientAsync(project).ConfigureAwait(false);

    var files = await projectClient.Files
        .GetFolderItemsAsync(project.RootFolderIdentifier)
        .ConfigureAwait(false);
}
```

Do not call the obsolete `InitializeTrimbleConnectUserAsync(string accessToken)` overload, and do not set `TrimbleConnectUser.AuthenticationToken`. Both of those paths are Bearer-only.

### Initialize PSet, Topic, Model, Issue, and ECom clients

Pass the **same** `DpopCredentialProvider` instance.

```csharp
using Trimble.Connect.PSet.Client;
using Trimble.Connect.Topic.Client;
using Trimble.Connect.Model.Client;
using Trimble.Connect.Model.Client.Implementation;
using Trimble.Connect.Issue.Client;
using Trimble.Connect.EComService.Client;

var psetClient = new PSetClient(
    new PSetClientConfig { ServiceURI = psetServiceUri },
    provider);

var topicClient = new TopicServiceClient(
    new TopicServiceClientConfig { ServiceURI = topicServiceUri },
    provider);

var modelClient = new ModelServiceClient(
    new ModelServiceClientConfig { ServiceURI = modelServiceUri },
    provider,
    files: projectClient.Files);

var issueClient = new IssueServiceClient(
    new IssueServiceClientConfig { ServiceURI = issueServiceUri },
    provider);

var ecomClient = new TrimbleConnectEComServiceClient(
    new EComServiceClientConfig { ServiceURI = ecomServiceUri },
    provider);
```

PSet (and Org) can also be configured by region instead of `ServiceURI`:

```csharp
var psetConfig = new PSetClientConfig
{
    Environment = "STAGE",
    Region = "us-east-1",
};
var psetClient = new PSetClient(psetConfig, provider);
```

### Share one provider across clients

`ICredentialsProvider` is designed to be shared so token refresh is coordinated. Construct **one** `DpopCredentialProvider` per user/subject and pass it to every TC client in that request scope.

Do not create a new provider per HTTP call. Do not mix `DpopCredentialProvider` and `TokenCredentialsProvider` on clients that should act as the same subject.

### Regions configuration

The regions endpoint is hardcoded to `https://app.connect.trimble.com/tc/api/2.0/regions` in Common. For STAGE (or any non-production master):

```csharp
RegionsConfig.RegionsUri = new Uri("https://app.stage.connect.trimble.com/tc/api/2.0/regions");
await RegionsConfig.InitializeFromServerAsync(provider).ConfigureAwait(false);
```

`TrimbleConnectClient.ReadConfigurationAsync()` does this automatically when `CredentialsProvider` is set. PSet / Model clients that resolve URI from a Trimble Connect **project location** (for example `northAmerica`) still need `RegionsConfig` initialized first.

## Proof signing contract

`createProofAsync` receives the values the resource server will validate.

| Argument | Use in the proof |
|---|---|
| `method` | JWT claim `htm` (HTTP method, uppercase, for example `GET`). |
| `uri` | JWT claim `htu`. Use the absolute URI of the request the handler is about to send, including path and query, **without** fragment. |
| `token` | JWT claim `ath` — base64url SHA-256 of the ASCII access token. Also the value placed in `Authorization: DPoP`. |
| `nonce` | JWT claim `nonce` when the server sent `DPoP-Nonce`. Omit the claim when `nonce` is null. |

Typical JOSE header:

* `typ`: `dpop+jwt`
* `alg`: the algorithm of the DPoP key (commonly `ES256`)
* `jwk`: the public key that matches `cnf.jkt` on the access token

Typical payload:

* `jti`: unique per proof (never reuse across sends, including retries)
* `iat`: issued-at
* `htm`, `htu`, `ath`
* `nonce` when provided

Project Sight should reuse the signer already used for subject-delegation HTTP calls. Wire it as the `createProofAsync` delegate; do not implement a second DPoP stack.

The URI passed in is `HttpRequestMessage.RequestUri` after the client has built the request. Sign that URI. Do not sign a different host (for example an internal alias) than the client actually calls.

## Retry, nonce, and token renewal

`RetryHttpMessageHandler` authorizes **each physical send**. After `InvalidateAndAuthorizeAsync` prepares a retry, the handler sends that request as-is so a nonce proof is not overwritten.

| Server response | Provider behavior | Token renewed? |
|---|---|---|
| 2xx | none | No |
| 5xx or 429 (retryable, buffered body) | `AuthorizeAsync` again → new proof, same token | No |
| 401 with `WWW-Authenticate: DPoP error="use_dpop_nonce"` and `DPoP-Nonce` | new proof that includes the nonce | **No** |
| 401 with `DPoP-Nonce` but not a nonce challenge (for example `invalid_token`) | `renewAccessTokenAsync`, then a new proof that includes the nonce | Yes |
| Other 401 | `renewAccessTokenAsync`, then a new proof | Yes |
| 401 nonce challenge **without** `DPoP-Nonce` | throws `InvalidOperationException("The DPoP nonce challenge has no nonce.")` | No |
| `InvalidateAndAuthorizeAsync(null, _)` | `renewAccessTokenAsync` only | Yes |

Credentials are refreshed at most **once** per logical request. A second 401 is surfaced as `InvalidServiceOperationException`.

Nonce is applied to the **immediate retry only**. The provider does not cache `DPoP-Nonce` for later independent requests. If the resource server requires a nonce on every call after the first challenge, the next call may take one extra 401 round-trip. That is expected with the current Common implementation.

Requests marked skip-authorization (presigned file URLs, some thumbnails) send neither `Authorization` nor `DPoP`. A 401 on those requests attaches credentials once and retries; that path is for TC APIs, not for S3.

## What Project Sight must not do

1. **Do not wrap a DPoP access token in `TokenCredentialsProvider`.** That sets `Authorization: Bearer <token>`. A DPoP-bound token must use the `DPoP` scheme plus a proof.
2. **Do not use `AuthCodeCredentialsProvider` / `PasswordCredentialsProvider` for this path.** Those providers also set Bearer.
3. **Do not set `TrimbleConnectUser.AuthenticationToken`.** It is obsolete and Bearer-only. When a credentials provider is present, `GetUserAuthenticationToken()` already returns null.
4. **Do not implement a second `ICredentialsProvider` that adds DPoP unless it also handles nonce and proof replacement.** Prefer `DpopCredentialProvider`.
5. **Do not send DPoP proofs to presigned storage URLs.** The clients already call `SkipAuthorization()` for those downloads and uploads.
6. **Do not reuse proofs.** `AuthorizeAsync` is called again on every 5xx/429 retry specifically so a new `jti` is generated.

## File transfer and skip-authorization

Connect file download of content from object storage uses a presigned URL and `SkipAuthorization()`. Thumbnail download of an absolute HTTP URL does the same. DPoP is therefore **not** attached to S3/Data Ocean transfers, which is required because those URLs are not DPoP resource servers.

The **Trimble Connect API calls** that create upload sessions, request download URLs, list folders, and rename files go through the provider and **do** receive DPoP.

If a Project Sight flow still 401s on upload or bulk-file metadata, check whether that specific Connect endpoint has been enabled for DPoP on the server. The client will already be sending the correct headers once `DpopCredentialProvider` is wired.

## Code snippets

### Example: Project Sight adapter around an existing token cache

Keep TID token exchange and JWT signing in Project Sight code. The SDK only needs three functions.

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;
using Trimble.Connect.Client;

public sealed class ProjectSightDpopAdapter
{
    private readonly ISubjectDelegationTokens tokens;

    public ProjectSightDpopAdapter(ISubjectDelegationTokens tokens)
    {
        this.tokens = tokens;
    }

    public ICredentialsProvider CreateProvider()
    {
        return new DpopCredentialProvider(
            this.tokens.GetValidAccessTokenAsync,
            this.tokens.RenewAccessTokenAsync,
            this.CreateProofAsync);
    }

    private Task<string> CreateProofAsync(HttpMethod method, Uri uri, string accessToken, string nonce)
    {
        return this.tokens.CreateDpopProofAsync(
            httpMethod: method.Method,
            requestUri: uri,
            accessToken: accessToken,
            nonce: nonce);
    }
}

// Names below are illustrative. Map them onto Project Sight's existing
// subject-delegation token service and DPoP signer.
public interface ISubjectDelegationTokens
{
    Task<string> GetValidAccessTokenAsync();
    Task<string> RenewAccessTokenAsync();
    Task<string> CreateDpopProofAsync(string httpMethod, Uri requestUri, string accessToken, string nonce);
}
```

### Example: Access TC API endpoints with DPoP

```csharp
var client = new TrimbleConnectClient(config, provider);
await client.InitializeTrimbleConnectUserAsync().ConfigureAwait(false);

var projects = await client.GetProjectsAsync().ConfigureAwait(false);
var project = projects.First();
var projectClient = await client.GetProjectClientAsync(project).ConfigureAwait(false);

var todos = await projectClient.Todos.GetAllAsync().ConfigureAwait(false);
var files = (await projectClient.Files
    .GetFolderItemsAsync(project.RootFolderIdentifier)
    .ConfigureAwait(false)).ToArray();
```

### Example: PSet with the same DPoP provider

```csharp
var psetConfig = new PSetClientConfig
{
    ServiceURI = new Uri("https://pset-api.us-east-1.stage.connect.trimble.com/v1/"),
};
var psetClient = new PSetClient(psetConfig, provider);

var library = await psetClient.GetLibraryAsync(new GetLibraryRequest
{
    LibraryId = libraryId,
}).ConfigureAwait(false);
```

### Example: Reuse credentials from Connect Client in Model Client

```csharp
using (var tcClient = new TrimbleConnectClient(tcConfig, provider))
{
    await tcClient.ReadConfigurationAsync().ConfigureAwait(false);
    await tcClient.InitializeTrimbleConnectUserAsync().ConfigureAwait(false);

    var projectClient = await tcClient.GetProjectClientAsync(project).ConfigureAwait(false);

    using (var modelClient = new ModelServiceClient(
        new ModelServiceClientConfig { ServiceURI = modelServiceUri },
        tcClient.CredentialsProvider,
        projectClient.Files))
    {
        var model = await modelClient.GetModelAsync(new GetModelRequest
        {
            ModelId = modelId,
            VersionId = versionId,
        }).ConfigureAwait(false);
    }
}
```

## Troubleshooting

| Symptom | Likely cause | What to do |
|---|---|---|
| `Authorization` is `Bearer` | `TokenCredentialsProvider` or Identity AuthCode/Password provider is still passed into the client. | Construct clients with `DpopCredentialProvider` only. |
| 401 immediately, no `DPoP` header | Process is still running Common earlier than **2.1.0**, or skip-authorization was set. | Add a direct PackageReference on `Trimble.Connect.Client.Common` 2.1.0; confirm `DpopCredentialProvider` is the type passed to the constructor. |
| 401 `use_dpop_nonce` then still 401 | Proof retry dropped the nonce, or `createProofAsync` ignored the nonce argument. | Confirm Common includes the retry-handler change; include `nonce` in the proof payload when it is not null. |
| 401 after a 5xx retry | Proof `jti` was reused. | Ensure `createProofAsync` generates a new `jti` on every call. |
| `The DPoP access token is missing.` | Token delegate returned null/empty. | Fix token cache / token exchange before calling the client. |
| `The DPoP proof is missing.` | Proof delegate returned null/empty. | Fix the signer. |
| `A DPoP request needs an absolute URI.` | Request URI was relative. | Should not happen with the shipped clients; if you build custom requests, use an absolute URI. |
| `Cannot refresh immutable token` | A `TokenCredentialsProvider` received a 401. | Switch to `DpopCredentialProvider`. |
| 401 only on some endpoints (bulk file metadata, upload, rename) | Those Connect APIs may not accept DPoP yet. | Keep those calls on the previous auth path until the server rollout includes them. |
| Proof `htu` rejected | Signer used a different URI than `HttpRequestMessage.RequestUri` (scheme, host, path, or query). | Sign the `uri` argument as given. |

Inspect the first request on the wire:

```
Authorization: DPoP eyJ...
DPoP: eyJ...
```

There must be exactly one `DPoP` header value per send. `DpopCredentialProvider` removes the previous proof before adding the new one when the same `HttpRequestMessage` is authorized again.

## Related guides

* [Developer Guide.md](https://github.com/trimble-oss/tc-dotnet-sdk-docs/blob/main/documentation/Developer%20Guide.md) — SDK components and environments
* [Developer Guide - Client.md](https://github.com/trimble-oss/tc-dotnet-sdk-docs/blob/main/documentation/Developer%20Guide%20-%20Client.md) — `Trimble.Connect.Client`
* [Developer Guide - PSet.md](https://github.com/trimble-oss/tc-dotnet-sdk-docs/blob/main/documentation/Developer%20Guide%20-%20PSet.md) — Property Set client
* [Developer Guide - Topic Client.md](https://github.com/trimble-oss/tc-dotnet-sdk-docs/blob/main/documentation/Developer%20Guide%20-%20Topic%20Client.md) — Topic client
* [Developer Guide - Model Client.md](https://github.com/trimble-oss/tc-dotnet-sdk-docs/blob/main/documentation/Developer%20Guide%20-%20Model%20Client.md) — Model client
* [Developer Guide - Org.md](https://github.com/trimble-oss/tc-dotnet-sdk-docs/blob/main/documentation/Developer%20Guide%20-%20Org.md) — Organization client
* [Developer Guide - EComClient.md](https://github.com/trimble-oss/tc-dotnet-sdk-docs/blob/main/documentation/Developer%20Guide%20-%20EComClient.md) — ECom client
* [Developer Guide - Identity_OAuth_AuthCode.md](https://github.com/trimble-oss/tc-dotnet-sdk-docs/blob/main/documentation/Developer%20Guide%20-%20Identity_OAuth_AuthCode.md) — interactive Bearer sign-in (not used for DPoP)
