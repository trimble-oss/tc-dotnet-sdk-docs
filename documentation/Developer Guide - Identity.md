# Trimble.Identity .NET SDK Developer Guide

### Content

1. [Overview](#overview)
2. [Authentication flow](#flow)
3. [Authentication with Trimble Identity](#identity)
4. [Code tracing](#tracing)
5. [Code snippets](#snippets)
   + [Example: How to acquire security token for the first time](#acquire_token)
   + [Example: How to handle exceptions](#handle_exceptions)
   + [Example: How to cache security token](#cache_token)
   + [Example: How to refresh security token](#refresh_token) 

### Acronyms

TID - Trimble Identity

## <a name="overview">Overview</a>

All of Trimble Connect's API endpoints are secured, and as such require users to be authenticated.

The authentication component provides the authentication context for all TC API calls.  It communicates with the _Identity_ _Provider_, handles token acquisition protocols, token caching, token refresh, and user interaction.

The main concepts in the identity component are: **AuthenticationContext**, **AcquireToken**, **AuthenticationResult**, **TokenCache**. 

![Class diagram](images/Trimble.Identity_ClassDiagram.png)

**AuthenticationContext** represents a connection to the Identity Provider.  The **AuthenticationContext** has a set of **AcquireToken** overloaded methods to cover several authentication scenarios for headless and GUI applications.  For headless apps, the app must pass the **ICredentials** provider as a parameter, for GUI applications, the **AcquireToken** method is invoked via a web browser window displayed when and if user interaction is needed. 

**AuthenticationContext** has an associated persistent **TokenCache**.  The cache can be enumerated by the app if needed, to find previously acquired tokens.  **AcquireToken** methods are aware of the **TokenCache** and use cached tokens instead of initiating network communication whenever they are available. 

Acquired tokens are represented as **AuthenticationResult** instances and stored in the **TokenCache** indexed by **userId**.  **AuthenticationResult** is a set of properties available from the Identity Provider: access token, id token, user id, user display id, issues id, refresh token, expiration time and potentially other claims.

The main goal of the **TokenCache** is to minimize the number of user prompts and maximize the performance when acquiring tokens.  The cache is designed to work fully transparently for the application: **AuthenticationContext** uses tokens from the cache automatically when possible and silently refreshes those when needed.

Normally, an app doesn???t need to access the token cache directly, but there is also a direct API available for advanced scenarios.

The persistence mechanism for the cache is built to be pluggable.  The default implementation for the cache persistence is available out of the box, but can be overridden by the application to implement advanced caching and locking strategies, i.e web applications. 

The implementation and interfaces directly reflect what is available from Trimble Identity/Trimble Connect Services, basically [OpenID Connect](http://openid.net/connect/) (OAuth2 based) protocol without changing concepts, but just wrapping the protocol to easy to use interface with supplementary features like token caching, UI for user authentication experience, and tools for acquiring and refresh tokens.

The component assures that user credentials are never stored on the client device.  Tokens are cached in memory and persisted in the _TokenCache_ if and only if an app chooses to do so.

## <a name="flow">Authentication flow</a>

In a .NET client app, is is highly recommended that you use **AuthenticationContext** to acquire a **TID access token** and **id token**.  **AuthenticationContext** is the main class representing the token issuing authority for TID resources.  

**AuthenticationContext** does the following:

1. Starts the authentication flow by redirecting the user agent to the TID authorization endpoint.  The user authenticates and consents, if consent is required.

2. The TID authorization endpoint redirects the user agent back to the **AuthenticationContext** with an authorization code. The user agent returns an authorization code to the client application???s redirect URI.

3. The **AuthenticationContext** requests an access token from the TID token issuance endpoint.  It then presents the authorization code to prove that the user has consented.

4. The TID token issuance endpoint returns an **access token** together with an **id token** and a **refresh token**.

5. The client application uses the **access token** to authenticate to the TID API.

## <a name="identity">Authentication with Trimble Identity</a>

In order to use TID authentication, the calling app must be registered with Trimble.  This can be initiated by contacting the Trimble Connect integration team at [connect-integrate@trimble.com](mailto:connect-integrate@trimble.com). 

As a result of registration, the requesting developer will receive the following OAuth2 parameters: client credentials (id and secret) and returnUrl (usually http://localhost for native apps).  These must be passed as additional parameters to acquire the token. 

TID requires the tenant name in addition to the username.  The tenant name can be passed as a separate parameter to the NetworkCredential constructor or alternatively the user name can be formatted together with the tenant (UPN): `<name>@<tenant>`.

## <a name="tracing">Code tracing</a>

Tracing is implemented using [Trimble.Diagnostics](Developer%20Guide%20-%20Diagnostics.md) library.

The _Trimble.Identity_ component uses a similar approach for code instrumentation and tracing as the .NET network tracing (see [https://msdn.microsoft.com/en-us/library/ty48b824(v=vs.110).aspx](https://msdn.microsoft.com/en-us/library/ty48b824(v=vs.110).aspx)).

On desktop platforms, in order to enable tracing for **Trimble.Identity** trace source, please include the following in your app.config:
 
    <system.diagnostics>
    <sources>
        <source name="Trimble.Identity"></source>
    </sources>

    <switches>
      <add name="Trimble.Identity" value="Verbose"/>
    </switches>
    </system.diagnostics>

### Code tracing on mobile platforms

On mobile platforms (iOS, Android), app.config level tracing configuration is not yet available.  To configure tracing on these platforms please use the _Trimble.Identity.Logging_ class.  It has a switch static property to configure the code tracing level based on platform.  By default, tracing is switched off (SourceLevels.Off).

    Trimble.Identity.Logging.Switch.Level = SourceLevels.Verbose;

## <a name="snippets">Code snippets</a>

### <a name="acquire_token">Example: How to acquire security token for the first time</a>

In order to acquire an initial security token, an application must present its credentials (**ClientCredentials**) as well as user credentials (**ICredentials**, **NetworkCreadential**) when calling **AcquireTokenAsync**.

The code snippet below demonstrates how to initialize the AuthenticationContext and call _AcquireTokenAsync_:

    var clientCredential = new ClientCredential(<id>, <secret>, <appname>);
    var authCtx = new AuthenticationContext(clientCredential) { AuthorityUri = ...};
    var token = await authCtx.AcquireTokenAsync();

For a non-interactive authentication (Trimble-internal use only), the following can be passed as a parameter to _AcquireTokenAsync_:

    var userCredentials = new NetworkCredential(<username>, <pass>, <tenant>);

### <a name="handle_exceptions">Example: How to handle exceptions</a>

This example shows exception handling and how to recognize different error cases when calling AquireTokenAsync.

    try
    {
        return await authContext.AcquireTokenAsync();
    }
    catch (TaskCanceledException ex)
    {
        // handle network connectivity errors
        throw;
    }
    catch (AuthenticationException ex)
    {
        if (ex.ErrorCode == ErrorCode.InvalidGrant)
        {
            // wrong credentials, show message to user
        }
        throw;
    }
    catch (TokenRefreshException ex)
    {
        // no network connectivity, but there is a token in the cache
        return ex.ExpiredToken;
    }

### <a name="cache_token">Example: How to cache security token</a>

Typically the application would want to cache the acquired security token and store it in persistent storage.  This way, a user's identity can be restored after the application is restarted.  Please note that an app should not store user credentials, the security token should be used for this purpose as a more secure mechanism.

    string t = token.Serialize();            
    // store the token serialized to the string somewhere
    ...
    // restore token on app startup
    token = AuthenticationResult.Deserialize(t);

### <a name="refresh_token">Example: How to refresh security token</a>

The access token periodically expires.  The expiration time is controlled by the token issuer.  A new token can be acquired using user credentials or by refreshing the existing token.  In the latter case, user credentials are not required.

    token = await authCtx.AcquireTokenByRefreshTokenAsync(token);

