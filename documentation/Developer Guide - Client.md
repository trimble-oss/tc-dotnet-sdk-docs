# Trimble.Connect.Client .NET SDK Developer Guide

### Content

1. [Overview](#overview)
2. [Authentication](#authentication)
	+ [Example: Sign-in TC Platform Services](#sign-in-example)
	+ [Example: Use retry handler to refresh expired access token automatically](#refresh-token-example)
3. [TC Data Model](#data-model)
4. [Accessing TC Platform Services](#accessing-tc)
5. [Extensibility mechanisms](#extensibility-mechanisms)
6. [Code snippets](#tc_snippets)
	+  [Example: Access TC Services](#example-access-tc)
	+  [Example: Using delegating handlers to implement network activity indicator](#example-delegating-handlers)

### Acronyms

TID - Trimble Identity, identity provider

TC - Trimble Connect

## <a name="overview">Overview</a>

The Trimble.Connect.Client component represents a set of TC Platform Services REST API wrappers. All TC APIs are provided for use by connected applications as _wrappers_. Wrappers provide full create, read, update, delete (CRUD) functionality against TC entities. Wrappers work on the same abstraction level as the web API and mostly map RESTful API requests one-to-one into .NET HTTP requests.

This layer's responsibilities include:

* Consistent error handling and convenient ways to deal with varying network performance (like async mechanisms and progress reporting).
* Providing an easy to use abstraction for your targeted platforms and programming language.
* Supporting middleware message handlers
* Leveraging strongly typed entity recognition for data modeling
* Graceful error handling
* Adherence to HTTP conventions and standards

The TC REST API specification can be found here: 

* Staging: [https://app.stage.connect.trimble.com/tc/static/apidoc.html](https://app.stage.connect.trimble.com/tc/static/apidoc.html) 

* Production: [https://app.connect.trimble.com/tc/static/apidoc.html](https://app.connect.trimble.com/tc/static/apidoc.html) 

The video tutorial: [SDK Authentication using the Identity library](https://www.youtube.com/watch?v=7Rkw117bfsA).

## <a name="overview">Authentication</a>

TC uses token based authentication. The token is issued by TC though exchanging a valid TID token acquired during the TID OAuth2 (OpenID Connect) authentication (see [Trimble.Identity Developer Guide](Trimble.Identity%20Developer%20Guide.md)).

It is important to understand that the ID token is generally a one use token with short life span. It is expired much faster than the TID access token and TC access token. This means that when the TC token is expired and a new TID token needs to be acquired (there is no refresh token concept for TC) app will have to request a new id token from TID.  This can be done by `AuthenticationContext.AcquireTokenByRefreshTokenAsync` or by using `RefreshOptions.IdToken` option in other `AuthenticationContext.AcquireTokenAsync` methods. Please refer to the [Trimble.Identity Developer Guide](Trimble.Identity%20Developer%20Guide.md).

### <a name="example-sign-in">Example: Authenticating with TC API </a>

To authenticate with TC, the TID token must be "exchanged" for a TC token.  Note that the ID token is usually a short lived. This basically means that the token is meant for one time use. `LoginAsync` could fail if the `id_token` is expired. 

    var authCtx = new AuthenticationContext(...);
    var accessToken = await authCtx.AcquireTokenAsync(RefreshOptions.IdToken)

    var serviceUri = ...;
    using (var client = new TrimbleConnectClient(serviceUri))
    {    
        await client.LoginAsync(accessToken.IdToken);        
        ...
    }

### <a name="example-refresh-token">Example: Use retry handler to refresh expired access token automatically</a>

The TC access token can expire at any moment of time that cannot be predicted by the application. To handle the token expiration it is recommended to use an http delegating handler (`RetryHandler`) that intercepts responses from the backend, handle access token refresh logic and repeat the original request transparently for the caller. 

Below is a code snippet showing how to install a `RetryHandler` and connect it to the TC client.

    var authCtx = new AuthenticationContext(...);
    
    var token = await authCtx.AcquireTokenAsync(...);
    var handler = new RetryHandler();
    
    using (var client = new TrimbleConnectClient(ServiceUri, handler))
    {
        handler.OnFailed = async (response, request, cancellationToken) =>
        {
            if (response.StatusCode == HttpStatusCode.Unauthorized)
            {
                var e = await InvalidServiceOperationException.FromResponse(response);
                if (e.ErrorCode == ResponseErrorCode.TidTokenExpired || 
                    e.ErrorCode == ResponseErrorCode.AccessTokenExpired)
                {    
                    token = await authCtx.AcquireTokenByRefreshTokenAsync(token);
    
                    await client.LoginAsync(token.IdToken, cancellationToken);
    
                    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", client.CurrentUser.AuthenticationToken);
    
                    return true;
                 }
            }
    
            return false;
        };
    
        await client.LoginAsync(token.IdToken);    
    
        // use the client normally for making service requests.    
    }

An application should be prepared such that `AcquireTokenByRefreshTokenAsync()` can fail because when a refresh token is revoked or invalid, in which case the token must be reacquired using the user credentials.

## <a name="data-model">TC Data Model</a>

![Data model](images/tcps_data_model.png)

## <a name="accessing-tc">Accessing the TC API</a>

Below is a class diagram that describes the API programming models used in the SDK component to access the TC service.

![Class diagram](images/class_diagram.png)

## <a name="extensibility-mechanisms">Extensibility mechanisms</a>

The SDK client component is designed to be backward and forward compatible. This means that applications released with older versions of the SDK should be still functional when new TC Service APIs release. Also if by some reason app developers don't want to migrate to newer versions of the SDK but need access to new TC Service APIs this can be done as well by using extension mechanisms.

The following mechanisms are used to achieve this flexibility:

1. Each entity has a `Properties` attribute implemented as a generic dictionary. This is a collection of "unknown" or "open" properties. E.g. if a new property is added to the entity after SDK has been released app can access such property from this dictionary.
2. We prefer string literals to enums. This makes it possible to deserialize new values even if they were not known at the time of SDK compilation.
3. All updates to entities are done using PATCH requests instead of PUT.
4. There is a generic mechanism to pass additional query parameters to the `GetAllAsync` and `SearchAsync` methods
5. There is a generic mechanism to invoke any TC API method in a dynamic way: `InvokeApiAsync`.
6. Application developer can provide custom `HttpMessageHandler` and `DelegatingHandlers` when creating `TrimbleConnectClient`. This is very powerfull mechanism to implement custom processing in the http pipeline. One example can be found [below](#example-delegating-handlers).

## <a name="tc_snippets">Code Snippets</a>
Below are some examples on using the Trimble Connect Client component.
Full sample applications can be found on [github](https://github.com/Trimble-Connect/samples)

### <a name="example-access-tc">Example: Access TC API endpoints</a>

    // list all projects
    var projects = await client.GetProjectsAsync();
    var project = projects.First();
    
    // get project specific wrapper
    var projectClient = await client.GetProjectClientAsync(project);
    
    // working with todos
    var todos = await projectClient.Todos.GetAllAsync();
    
    // working with files
    var files = (await projectClient.Files.GetFolderItemsAsync(project.RootFolderIdentifier)).ToArray();

    var file = files.FirstOrDefault(f => f.Type == EntityType.File);
    var stream = await projectClient.Files.DownloadAsync(file.Identifier);
    var destPath = Path.GetTempFileName();

    using (var destination = File.Create(destPath))
    {
        await stream.CopyToAsync(destination);
    }

### <a name="example-delegating-handlers">Example: Using delegating handlers to implement network activity indicator</a>

The code snippet below demonstrates how to use http delegating handlers to implement network activity indicator in the app.

    public class BusyHandler : DelegatingHandler
    {
        private int _callCount;
        private readonly Action<bool> _busyIndicator;
        
        public BusyHandler(Action<bool> busyIndicator)
        {
            _busyIndicator = busyIndicator;
        }
        
        protected override async Task<HttpResponseMessage> SendAsync(
          HttpRequestMessage request, CancellationToken cancellationToken)
        {
             // update the count by one in a single atomic operation. 
             // If we get a 1 back, we know we just went 'busy'
             var outgoingCount = Interlocked.Increment(ref _callCount);
             if (outgoingCount == 1)
             {
                 _busyIndicator(true);
             }
             
             var response = await base.SendAsync(request, cancellationToken);
             
             // decrement the count by one in a single atomic operation.
             // If we get a 0 back, we know we just went 'idle'
             var incomingCount = Interlocked.Decrement(ref _callCount);
             if (incomingCount == 0)
             {
                 _busyIndicator(false);
             }
             
             return response;
        }
    }

