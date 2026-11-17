# Trimble Connect .NET SDK Developer Guide — mTLS

### Content

1. [Overview](#overview)
2. [Who this is for](#can-an-application-use-mtls)
3. [How mTLS is attached](#how-mtls-is-attached)
4. [Prerequisites](#prerequisites)
5. [Authentication / Initialization](#authentication)
   + [Load the client certificate](#load-certificate)
   + [Create an `MtlsCredentialProvider`](#create-provider)
   + [Initialize Trimble Connect Client](#init-connect)
   + [Initialize PSet, Topic, Model, Issue, and ECom clients](#init-other)
   + [Share one provider across clients](#share-provider)
   + [Regions configuration](#regions-config)
6. [Token request contract](#token-request)
7. [Cache, refresh, and 401 retry](#cache-and-retry)
8. [What the application must not do](#must-not)
9. [File transfer and skip-authorization](#file-transfer)
10. [Code snippets](#snippets)
11. [Troubleshooting](#troubleshooting)
12. [Related guides](#related-guides)

### Acronyms

**TID** — Trimble Identity

**TC** — Trimble Connect

**mTLS** — Mutual TLS. The client presents an X.509 certificate during the TLS handshake.

**FedRAMP** — The U.S. Federal Risk and Authorization Management Program. This guide covers Trimble's internal FedRAMP Connect endpoints only.

**TLS client authentication** — TID accepts that certificate instead of a client secret on the token request (`tls_client_auth`).

## <a name="overview"></a>Overview

**This path is for internal FedRAMP endpoints only. Commercial Connect applications must not use `MtlsCredentialProvider`.** Commercial apps continue to use interactive authorization code grant, a caller-supplied bearer token, or DPoP as documented in the other developer guides.

Certificate authentication is available from **`Trimble.Connect.Client.Common` 2.2.0**. Earlier Common versions do not include `MtlsCredentialProvider`. See [ReleaseNotes-ClientCommon.md](https://github.com/trimble-oss/tc-dotnet-sdk-docs/blob/main/ReleaseNotes/ReleaseNotes-ClientCommon.md).

Trimble Connect .NET service clients authenticate through `ICredentialsProvider`. `MtlsCredentialProvider` is an `ICredentialsProvider` for an **internal FedRAMP** service that already has a Trimble ID client certificate and private key, and that calls **internal FedRAMP Connect endpoints**.

The provider:

* Posts `grant_type=client_credentials` to the FedRAMP mutual TLS token endpoint with `client_id` and `scope`. The body has no `client_secret`. The client certificate authenticates the application (`tls_client_auth`).
* Caches the access token and attaches it as `Authorization: Bearer` on those Connect calls.
* Refreshes the cached token before it expires, and again after HTTP 401. The retry is sent with the refreshed bearer token.
* Presents the same certificate on the TID token request and on Connect calls made with this provider when `ClientConfig.InnerHandler` is left unset.

The application loads the `X509Certificate2` (certificate and private key) and owns its lifetime. This library does not read PEM files, PFX files, or certificate signing requests.

Pass the mutual TLS token URL for the **FedRAMP** environment. Read `mtls_endpoint_aliases.token_endpoint` from that environment's OpenID discovery document. `GovernmentTokenEndpoint` is the current internal alias:

| Environment | Constant | Current token URL |
|---|---|---|
| FedRAMP (internal) | `GovernmentTokenEndpoint` | `https://tid-nlb.tid-fr.trimbleinternal.us/oauth/token` |

Prefer the discovered token endpoint when the alias changes. The type also exposes `ProductionTokenEndpoint` and `StagingTokenEndpoint`. Do not use them here; they are not the FedRAMP internal token URL.

Point `TrimbleConnectClientConfig.ServiceURI` (and other service URIs) at the **internal FedRAMP Connect API**, not `https://app.connect.trimble.com` or `https://app.stage.connect.trimble.com`.

## <a name="can-an-application-use-mtls"></a>Who this is for

**Internal FedRAMP services only.** A commercial Connect application must not use this provider. Use [AuthCode](https://github.com/trimble-oss/tc-dotnet-sdk-docs/blob/main/documentation/Developer%20Guide%20-%20Identity_OAuth_AuthCode.md), `TokenCredentialsProvider`, or [DPoP](https://github.com/trimble-oss/tc-dotnet-sdk-docs/blob/main/documentation/Developer%20Guide%20-%20DPoP.md) instead.

For a FedRAMP service, you do not need a fork of `Trimble.Connect.Client`, `Trimble.Connect.PSet.Client`, or the other service wrappers.

Every current TC .NET service client already takes `ICredentialsProvider` and builds its HTTP pipeline with `TrimbleConnectHttpClient.Create(config, credentialsProvider)`. Passing `MtlsCredentialProvider` is enough for those wrappers to send the bearer token from the certificate grant. When `InnerHandler` is unset, those internal Connect calls also present the client certificate.

| Client package | Constructor | Works with `MtlsCredentialProvider` |
|---|---|---|
| `Trimble.Connect.Client` | `TrimbleConnectClient(TrimbleConnectClientConfig, ICredentialsProvider)` | Yes. `GetProjectClient` / `GetProjectClientAsync` reuse the same HTTP pipeline. |
| `Trimble.Connect.PSet.Client` | `PSetClient(PSetClientConfig, ICredentialsProvider)` | Yes. |
| `Trimble.Connect.Topic.Client` | `TopicServiceClient(TopicServiceClientConfig, ICredentialsProvider)` | Yes. Pass the same provider into `GetProjectClient`. |
| `Trimble.Connect.Model.Client` | `ModelServiceClient(ModelServiceClientConfig, ICredentialsProvider, IFilesController)` | Yes. Reuse the Connect client's provider (and optionally `projectClient.Files`). |
| `Trimble.Connect.Issue.Client` | `IssueServiceClient(IssueServiceClientConfig, ICredentialsProvider)` | Yes. |
| `Trimble.Connect.EComService.Client` | `TrimbleConnectEComServiceClient(EComServiceClientConfig, ICredentialsProvider)` | Yes. |
| `Trimble.Connect.Org.Client` | `OrgClient(OrgClientConfig, ICredentialsProvider)` | Yes. |

Two conditions must both be true:

1. **Runtime Common package** — the process must load **`Trimble.Connect.Client.Common` 2.2.0 or later**, which contains `MtlsCredentialProvider` and the Connect handler that presents the certificate. Service client packages may pin an older Common. Add a **direct** `PackageReference` on Common 2.2.0 so NuGet does not keep the older assembly.
2. **FedRAMP TID must accept this client certificate** — the SDK sends the certificate and the client-credentials body. The certificate, client id, and scope must already be registered for TLS client authentication in the FedRAMP environment. Commercial TID registration is not this path.

`AuthCodeCredentialsProvider`, `TokenCredentialsProvider`, and `DpopCredentialProvider` do not present a client certificate. Use `MtlsCredentialProvider` only for this FedRAMP client-credentials path. Use `DpopCredentialProvider` only for a DPoP-bound delegated token. See [Developer Guide - DPoP.md](https://github.com/trimble-oss/tc-dotnet-sdk-docs/blob/main/documentation/Developer%20Guide%20-%20DPoP.md).

## <a name="how-mtls-is-attached"></a>How mTLS is attached

```
Application
  │  loads X509Certificate2 (certificate + private key)
  │  owns that object
  ▼
MtlsCredentialProvider  (ICredentialsProvider)
  │  token POST with the client certificate
  │  grant_type=client_credentials, client_id, scope
  │  caches access_token
  ▼
TrimbleConnectClient / PSetClient / TopicServiceClient / ...
  │  TrimbleConnectHttpClient.Create(config, provider)
  │  InnerHandler left unset → same client certificate on Connect calls
  ▼
RetryHttpMessageHandler
  │  before each send: AuthorizeAsync
  │    Authorization: Bearer <token>
  │  on 401: InvalidateAndAuthorizeAsync, one new token, retry once
  │  on 5xx/429: retry per RetryConfig with the cached bearer token
  ▼
Trimble Connect APIs
```

When `TrimbleConnectClient` is constructed with a credentials provider, `GetUserAuthenticationToken()` returns null. Controllers still call `CreateRequest(..., GetUserAuthenticationToken())`, but `WithAuthBearer(null)` is a no-op. `RetryHttpMessageHandler` then attaches `Authorization: Bearer` immediately before the request goes on the wire.

Newer clients (PSet, Topic, Model, Issue, ECom) never set Bearer themselves. They call `CreateRequest(method, uri)` and rely entirely on the provider.

Leave `ClientConfig.InnerHandler` unset. `TrimbleConnectHttpClient.Create` then installs a handler that presents this certificate. If the application sets `InnerHandler`, that handler is left as it is and Connect calls do not receive the certificate from this provider. The certificate handler decompresses gzip and deflate responses, matching the default Connect client.

## <a name="prerequisites"></a>Prerequisites

1. A Trimble ID client certificate that includes its private key, registered for TLS client authentication **in FedRAMP**.
2. The OAuth client id and scope registered for that internal application. Use the scope FedRAMP TID issued for the Connect APIs this process will call.
3. The FedRAMP mutual TLS token endpoint. Prefer OpenID discovery (`mtls_endpoint_aliases.token_endpoint`). `GovernmentTokenEndpoint` is the current alias.
4. The internal FedRAMP Connect API base URL (and any regional PSet / Model / Topic URIs). Do not use commercial `app.connect.trimble.com` hosts.
5. NuGet packages:
   * `Trimble.Connect.Client.Common` **2.2.0** or later.
   * The service client packages the application already uses (`Trimble.Connect.Client`, `Trimble.Connect.PSet.Client`, and so on).
6. A direct `PackageReference` on Common **2.2.0** so it wins over an older Common version declared by the service clients:

```xml
<ItemGroup>
  <PackageReference Include="Trimble.Connect.Client" Version="2.8.11" />
  <PackageReference Include="Trimble.Connect.PSet.Client" Version="..." />
  <!-- Direct reference so MtlsCredentialProvider and the certificate handler are loaded. -->
  <PackageReference Include="Trimble.Connect.Client.Common" Version="2.2.0" />
</ItemGroup>
```

The FedRAMP token endpoint is typically not signed by the machine trust store. Have those extra root certificates ready and pass them as `extraRoots`. They apply only to the token request. The server hostname is still verified.

## <a name="authentication"></a>Authentication / Initialization

### <a name="load-certificate"></a>Load the client certificate

The application loads the certificate. The object must include the private key (`HasPrivateKey` is true). The application keeps the object alive for as long as the provider uses it, and disposes it afterward.

.NET 5 and later, from PEM files:

```csharp
using System.Security.Cryptography.X509Certificates;

using var certificate = X509Certificate2.CreateFromPemFile(
    "myapp_certificate.pem",
    "myapp_private_key.pem");
```

.NET Framework, from a PFX:

```csharp
using System.Security.Cryptography.X509Certificates;

using var certificate = new X509Certificate2(pfxPath, password);
```

A service that already holds the certificate and key can use the same loader it uses for other outbound mTLS calls and pass the resulting `X509Certificate2` here.

### <a name="create-provider"></a>Create an `MtlsCredentialProvider`

```csharp
using System;
using System.Security.Cryptography.X509Certificates;
using Trimble.Connect.Client;

using var credentials = new MtlsCredentialProvider(
    certificate,
    clientId,
    scope,
    MtlsCredentialProvider.GovernmentTokenEndpoint,
    extraRoots);
```

| Argument | Meaning |
|---|---|
| `certificate` | The client certificate. Must include a private key. The caller owns it. |
| `clientId` | The OAuth client id. Required. Empty or whitespace is rejected. |
| `scope` | The OAuth scope. Required. Empty or whitespace is rejected. |
| `tokenEndpoint` | An absolute `https` URI. For FedRAMP use `GovernmentTokenEndpoint` or the URL from that environment's OpenID discovery. |
| `extraRoots` | Optional. Extra trust anchors for the **token request** only. Null uses the machine trust store. The caller owns those certificates. A hostname mismatch is rejected. |

`Dispose` on the provider releases the token `HttpClient`. It does not dispose the certificate.

### <a name="init-connect"></a>Initialize Trimble Connect Client

This is the same shape as the [Client developer guide](https://github.com/trimble-oss/tc-dotnet-sdk-docs/blob/main/documentation/Developer%20Guide%20-%20Client.md), with `MtlsCredentialProvider` in place of `AuthCodeCredentialsProvider`.

```csharp
using System;
using System.Linq;
using System.Security.Cryptography.X509Certificates;
using System.Threading.Tasks;
using Trimble.Connect.Client;
using Trimble.Connect.Client.Common;

var extraRoots = new X509Certificate2Collection { extraRoot };

var config = new TrimbleConnectClientConfig
{
    ServiceURI = fedrampConnectApiUri,
};
config.RetryConfig = new RetryConfig { MaxErrorRetry = 3 };

using var certificate = X509Certificate2.CreateFromPemFile(
    "myapp_certificate.pem",
    "myapp_private_key.pem");
using var provider = new MtlsCredentialProvider(
    certificate,
    clientId,
    scope,
    MtlsCredentialProvider.GovernmentTokenEndpoint,
    extraRoots);

RegionsConfig.RegionsUri = fedrampRegionsUri;

using (var client = new TrimbleConnectClient(config, provider)
{
    CurrentUser = new TrimbleConnectUser("me"),
})
{
    await client.ReadConfigurationAsync().ConfigureAwait(false);

    var projects = await client.GetProjectsAsync().ConfigureAwait(false);
    var project = projects.First();
    var projectClient = await client.GetProjectClientAsync(project).ConfigureAwait(false);

    var files = await projectClient.Files
        .GetFolderItemsAsync(project.RootFolderIdentifier)
        .ConfigureAwait(false);
}
```

Leave `config.InnerHandler` unset so Connect calls present the certificate.

Do **not** call `InitializeTrimbleConnectUserAsync()` for this grant. That method loads `GET users/me`, which fails for an application / client-credentials token. Project wrappers such as `FilesController.GetFolderItemsAsync` still require `CurrentUser` (otherwise `ArgumentNullException`: parameter `user`). Stub it with `"me"` as in the sample above. `"Unknown"` also works. See [Application tokens and CurrentUser](https://github.com/trimble-oss/tc-dotnet-sdk-docs/blob/main/documentation/Developer%20Guide%20-%20Client.md#app-tokens-current-user).

PSet, Topic, Model, Issue, and ECom clients do not require `CurrentUser`.

Do not call the obsolete `InitializeTrimbleConnectUserAsync(string accessToken)` overload, and do not set `TrimbleConnectUser.AuthenticationToken`. Both of those paths are a caller-supplied bearer token. This provider supplies the bearer token itself.

### <a name="init-other"></a>Initialize PSet, Topic, Model, Issue, and ECom clients

Pass the **same** `MtlsCredentialProvider` instance. Leave each client's `InnerHandler` unset when those calls must present the certificate.

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

PSet and Org can also take an explicit FedRAMP `ServiceURI` (do not use commercial `Environment` / `Region` names):

```csharp
var psetConfig = new PSetClientConfig
{
    ServiceURI = fedrampPsetServiceUri,
};
var psetClient = new PSetClient(psetConfig, provider);
```

### <a name="share-provider"></a>Share one provider across clients

`ICredentialsProvider` is designed to be shared so token refresh is coordinated. Construct **one** `MtlsCredentialProvider` for the application certificate and pass it to every TC client that should use that identity.

Do not create a new provider per HTTP call. Do not mix `MtlsCredentialProvider` with `TokenCredentialsProvider` or `DpopCredentialProvider` on clients that should act as the same application.

### <a name="regions-config"></a>Regions configuration

The regions endpoint is hardcoded to commercial production (`https://app.connect.trimble.com/tc/api/2.0/regions`) in Common. For FedRAMP, point it at the **internal** regions URL before any client resolves a project location:

```csharp
RegionsConfig.RegionsUri = fedrampRegionsUri;
await RegionsConfig.InitializeFromServerAsync(provider).ConfigureAwait(false);
```

Do not leave the commercial default in place. `TrimbleConnectClient.ReadConfigurationAsync()` calls `InitializeFromServerAsync` when `CredentialsProvider` is set, so set `RegionsUri` first.

The FedRAMP regions response lists **external** endpoints. Do not infer internal FedRAMP URLs from it. If this service must call internal downstream APIs (PSet, Model, Topic, and so on), get those hosts from the **service owner** and set each client's `ServiceURI` explicitly. Using `region.TcpsApi` or other region fields for internals will hit the external edge.

## <a name="token-request"></a>Token request contract

The provider posts form fields to `tokenEndpoint`. The TLS handshake presents the client certificate. There is no client secret.

| Field | Value |
|---|---|
| `grant_type` | `client_credentials` |
| `client_id` | The `clientId` argument |
| `scope` | The `scope` argument |

The response must be JSON with `access_token`. `expires_in` is used for cache lifetime. Failures throw `InvalidOperationException` and **do not** include the response body, because it may contain secrets:

| Token response | Exception message |
|---|---|
| Non-success HTTP status | `The token endpoint rejected the client certificate request. HTTP {code}.` |
| Body is not valid JSON | `The token endpoint response was not valid.` |
| Missing `access_token` | `The token endpoint response did not include an access token.` |

`extraRoots`, when supplied, are copied and used only while validating the **token endpoint** certificate. They are not installed on Connect calls. The machine trust store is used when `extraRoots` is null or empty. A hostname mismatch is rejected. An unrelated root is rejected.

## <a name="cache-and-retry"></a>Cache, refresh, and 401 retry

One provider keeps one cached access token. Concurrent callers share an in-flight token request instead of posting twice.

| Situation | Behavior |
|---|---|
| Cached token is still inside its refresh window | `AuthorizeAsync` reuses it. No token POST. |
| Token lifetime is longer than 5 minutes | Refresh about 5 minutes before `expires_in`. |
| Token lifetime is 5 minutes or less | Keep it until the real expiry, then request a new one. |
| `expires_in` is missing or not positive | Treat it as already expired. The next call requests a new token. |
| HTTP 401 from Connect | `InvalidateAndAuthorizeAsync` discards the cache, posts for a new token, and the retry is sent with that bearer token. |
| HTTP 5xx or 429 | Retry per `RetryConfig`. The cached bearer token is sent again when it is still inside the refresh window. |
| Second 401 on the same logical request | Surfaced as `InvalidServiceOperationException`. Credentials are refreshed at most once per logical request. |

The scheme on Connect calls is `Bearer`. This provider does not send a DPoP proof.

## <a name="must-not"></a>What the application must not do

1. **Do not use this provider from a commercial Connect application.** It is only for internal FedRAMP endpoints. Commercial apps use AuthCode, `TokenCredentialsProvider`, or DPoP.
2. **Do not point `ServiceURI` at commercial Connect hosts** (`app.connect.trimble.com`, `app.stage.connect.trimble.com`, or commercial regional APIs).
3. **Do not ask this library to load PEM, PFX, or a certificate signing request.** Load the `X509Certificate2` in application code and pass that object.
4. **Do not pass a certificate that has no private key.** The constructor rejects it.
5. **Do not set `ClientConfig.InnerHandler` on a client that must present this certificate.** The SDK installs the certificate handler only when `InnerHandler` is null. Other credential providers are unchanged and do not get a client certificate.
6. **Do not put a client secret in the token request.** The grant is TLS client authentication. The provider does not send `client_secret`.
7. **Do not wrap this identity in `TokenCredentialsProvider` or `AuthCodeCredentialsProvider`.** Those providers send a bearer token and do not present the client certificate.
8. **Do not use `DpopCredentialProvider` for this path.** DPoP is a different grant: a delegated token plus a per-request proof. mTLS here is a FedRAMP client-credentials bearer token.
9. **Do not dispose the certificate while the provider is still in use.** The provider and every Connect client created from it keep using that object.
10. **Do not point `tokenEndpoint` at a non-HTTPS URL.** The constructor requires an absolute `https` URI.

## <a name="file-transfer"></a>File transfer and skip-authorization

Connect file download of content from object storage uses a presigned URL and `SkipAuthorization()`. Thumbnail download of an absolute HTTP URL does the same. Those requests do **not** receive `Authorization: Bearer`.

The **Trimble Connect API calls** that create upload sessions, request download URLs, list folders, and rename files go through the provider and **do** receive the bearer token. When `InnerHandler` is unset, the object client and the file client share a handler that can present the client certificate. A storage host that does not request a client certificate during the handshake does not receive one.

## <a name="snippets"></a>Code snippets

### Example: FedRAMP client from PEM files

```csharp
using System;
using System.Security.Cryptography.X509Certificates;
using Trimble.Connect.Client;

using var certificate = X509Certificate2.CreateFromPemFile(
    "myapp_certificate.pem",
    "myapp_private_key.pem");

var extraRoots = new X509Certificate2Collection { extraRoot };

using var provider = new MtlsCredentialProvider(
    certificate,
    clientId,
    scope,
    MtlsCredentialProvider.GovernmentTokenEndpoint,
    extraRoots);

RegionsConfig.RegionsUri = fedrampRegionsUri;

var config = new TrimbleConnectClientConfig
{
    ServiceURI = fedrampConnectApiUri,
};

using var client = new TrimbleConnectClient(config, provider)
{
    CurrentUser = new TrimbleConnectUser("me"),
};

await client.ReadConfigurationAsync().ConfigureAwait(false);
var projects = await client.GetProjectsAsync().ConfigureAwait(false);
```

### Example: .NET Framework PFX

```csharp
using System.Security.Cryptography.X509Certificates;
using Trimble.Connect.Client;

using var certificate = new X509Certificate2(pfxPath, password);
using var provider = new MtlsCredentialProvider(
    certificate,
    clientId,
    scope,
    MtlsCredentialProvider.GovernmentTokenEndpoint,
    extraRoots);
```

### Example: FedRAMP token endpoint with extra roots

```csharp
var extraRoots = new X509Certificate2Collection { extraRoot };

using var provider = new MtlsCredentialProvider(
    certificate,
    clientId,
    scope,
    MtlsCredentialProvider.GovernmentTokenEndpoint,
    extraRoots);
```

`extraRoots` is used for the token request. Connect calls still use the machine trust store. The token host name must match the certificate.

### Example: PSet with the same provider

```csharp
var psetConfig = new PSetClientConfig
{
    ServiceURI = fedrampPsetServiceUri,
};
var psetClient = new PSetClient(psetConfig, provider);

var library = await psetClient.GetLibraryAsync(new GetLibraryRequest
{
    LibraryId = libraryId,
}).ConfigureAwait(false);
```

### Example: reuse credentials from Connect Client in Model Client

```csharp
using (var tcClient = new TrimbleConnectClient(tcConfig, provider)
{
    CurrentUser = new TrimbleConnectUser("me"),
})
{
    await tcClient.ReadConfigurationAsync().ConfigureAwait(false);

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

## <a name="troubleshooting"></a>Troubleshooting

| Symptom | Likely cause | What to do |
|---|---|---|
| `ArgumentException`: the client certificate must include a private key | The `X509Certificate2` was loaded from a public certificate only, or the PFX password or key flags dropped the key. | Load the certificate and the private key together. Check `HasPrivateKey` before constructing the provider. |
| `ArgumentException`: a client id / scope is required | `clientId` or `scope` is empty or whitespace. | Pass the registered TID client id and scope. |
| `ArgumentException`: token endpoint must be an absolute https URI | `tokenEndpoint` is relative or not `https`. | Pass `GovernmentTokenEndpoint` or the FedRAMP discovery URL. |
| `InvalidOperationException`: token endpoint rejected the request (HTTP 4xx/5xx) | FedRAMP TID rejected the certificate grant. The exception does not include the response body. | Use `GovernmentTokenEndpoint` or `mtls_endpoint_aliases.token_endpoint` from FedRAMP OpenID discovery. Confirm the certificate, client id, and scope are registered for TLS client authentication in FedRAMP. |
| `InvalidOperationException`: token endpoint response was not valid / did not include an access token | The endpoint returned a non-token body or omitted `access_token`. | Confirm you called the FedRAMP mutual TLS token URL, not a commercial token endpoint. |
| `NotSupportedException`: this runtime cannot present a client certificate | `HttpClientHandler` has no `ClientCertificates` on this target. | Use a runtime that supports client certificates on `HttpClientHandler` (desktop .NET, current .NET, and the mobile targets Common ships). |
| `NotSupportedException`: this runtime cannot apply extra root certificates | `extraRoots` was passed but the handler has no custom validation callback. | Omit `extraRoots` when the machine trust store already includes the issuer, or use a runtime that supports the callback. |
| Token POST never presents a certificate | The certificate has no private key, or a different host than the mutual TLS token endpoint is being called. | Use the mTLS token URL from discovery. Confirm the registered certificate is the one passed to the constructor. |
| Connect calls do not present the certificate | `ClientConfig.InnerHandler` is set. | Leave `InnerHandler` unset and construct the client with `MtlsCredentialProvider`. |
| HTTP 401 from TID | The certificate, client id, or scope is not registered for TLS client authentication in FedRAMP, or a commercial token URL was used. | Use `GovernmentTokenEndpoint` / FedRAMP `mtls_endpoint_aliases.token_endpoint`. Confirm registration with FedRAMP TID. |
| Calls go to commercial Connect | `ServiceURI` or `RegionsUri` still points at `app.connect.trimble.com` / `app.stage.connect.trimble.com`. | Point both at the internal FedRAMP hosts. This provider is not for commercial apps. |
| HTTP 401 from Connect, then a second 401 | The refreshed token was also rejected. Refresh happens once per logical request. | Confirm the scope includes the Connect API being called. Inspect the second status as `InvalidServiceOperationException`. |
| `ArgumentNullException`: parameter `user` from `GetFolderItemsAsync` (or other paged wrappers) | `CurrentUser` is null. Application tokens have no Trimble Connect user profile. | Set `client.CurrentUser = new TrimbleConnectUser("me")` before `GetProjectClientAsync`. Do not call `InitializeTrimbleConnectUserAsync()`. |
| `InitializeTrimbleConnectUserAsync` fails after a successful token POST | The client-credentials token is not mapped to a Trimble Connect user. | Expected for application tokens. Stub `CurrentUser` with `"me"` instead. |
| Token host name mismatch | `extraRoots` cannot override a hostname failure. | Call the host name on the token certificate. |
| FedRAMP token request fails chain building | The FedRAMP issuer is not in the machine trust store. | Pass that issuer in `extraRoots`. |
| `Authorization` is `DPoP` or there is no bearer token | `DpopCredentialProvider` or a skip-authorization path is in use. | Use `MtlsCredentialProvider` for this grant. Presigned file URLs omit `Authorization` on purpose. |
| Process is missing `MtlsCredentialProvider` | Common older than **2.2.0** was loaded. | Add a direct `PackageReference` on `Trimble.Connect.Client.Common` 2.2.0. |

A successful Connect call carries:

```
Authorization: Bearer eyJ...
```

The token request is a POST with `grant_type=client_credentials`, `client_id`, and `scope`, and with the client certificate on the TLS handshake.

## <a name="related-guides"></a>Related guides

* [Developer Guide.md](https://github.com/trimble-oss/tc-dotnet-sdk-docs/blob/main/documentation/Developer%20Guide.md) — SDK components and environments
* [ReleaseNotes-ClientCommon.md](https://github.com/trimble-oss/tc-dotnet-sdk-docs/blob/main/ReleaseNotes/ReleaseNotes-ClientCommon.md) — Common 2.2.0 (`MtlsCredentialProvider`)
* [Developer Guide - DPoP.md](https://github.com/trimble-oss/tc-dotnet-sdk-docs/blob/main/documentation/Developer%20Guide%20-%20DPoP.md) — DPoP-bound delegated tokens (not this grant)
* [Developer Guide - Client.md](https://github.com/trimble-oss/tc-dotnet-sdk-docs/blob/main/documentation/Developer%20Guide%20-%20Client.md) — `Trimble.Connect.Client`
* [Developer Guide - PSet.md](https://github.com/trimble-oss/tc-dotnet-sdk-docs/blob/main/documentation/Developer%20Guide%20-%20PSet.md) — Property Set client
* [Developer Guide - Topic Client.md](https://github.com/trimble-oss/tc-dotnet-sdk-docs/blob/main/documentation/Developer%20Guide%20-%20Topic%20Client.md) — Topic client
* [Developer Guide - Model Client.md](https://github.com/trimble-oss/tc-dotnet-sdk-docs/blob/main/documentation/Developer%20Guide%20-%20Model%20Client.md) — Model client
* [Developer Guide - Org.md](https://github.com/trimble-oss/tc-dotnet-sdk-docs/blob/main/documentation/Developer%20Guide%20-%20Org.md) — Organization client
* [Developer Guide - EComClient.md](https://github.com/trimble-oss/tc-dotnet-sdk-docs/blob/main/documentation/Developer%20Guide%20-%20EComClient.md) — ECom client
* [Developer Guide - Identity_OAuth_AuthCode.md](https://github.com/trimble-oss/tc-dotnet-sdk-docs/blob/main/documentation/Developer%20Guide%20-%20Identity_OAuth_AuthCode.md) — interactive user sign-in for commercial apps. That path does not present this client certificate.
