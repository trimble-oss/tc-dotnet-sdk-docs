# Trimble.Connect.ECom.Client .NET SDK Developer Guide

## Content

1. [Overview](#overview)
2. [Authentication](#authentication)
3. [Code snippets](#code-snippets)
	+  [GetActivations API](#get-activations)
	+  [Example](#example)
	


### Overview

The current ECom Client component supports the GetActivations API [ ECom Service API swagger](https://ecom.stage.connect.trimble.com/v1/swagger-ui.html#/Activations) which lists the activations of the user. The activations will have the information about the licenses that the user owns.


### Authentication

TC uses token based authentication. The token is acquired during the TID OAuth2 (OpenID Connect) authentication (see [Trimble.ID Developer Guide](https://github.com/trimble-oss/trimble-id-sdk-docs-for-net)).

It is important to understand that the ID token is generally a one use token with short life span. This means that when the token is expired and a new TID token needs to be acquired. App will have to request a new id token from TID. This can be done by     
`AuthenticationContext.AcquireTokenByRefreshTokenAsync` or by using `RefreshOptions.IdToken` option
 in other `AuthenticationContext.AcquireTokenAsync` methods. Please refer to the [Trimble.ID Developer Guide](https://github.com/trimble-oss/trimble-id-sdk-docs-for-net).
 
The clients can use Trimble.Identity.OAuth.AuthCode for interactive workflow authentication or Trimble.Identity.OAuth.Password for headless applications (like from test code).
These newer components provide higher levels of abstraction and encapsulate the token acquiring and refreshing part.
The clients can also provide any custom implementation of ICredentialsProvider.

### Example: Authenticating and Initializing.

Authenticate with TID initialize and TrimbleConnectEComServiceClient. 


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
    var config = new TrimbleConnectClientConfig { ServiceURI = "https://ecom.connect.trimble.com/v1"};
    config.RetryConfig = new RetryConfig { MaxErrorRetry = 1 };
    using (var client = new TrimbleConnectEComServiceClient(config, provider))
    {    
        ...
    }
															
### Code Snippets

Below given is the example for using the Trimble Connect ECom Client component.

#### GetActivations API 

The GetActivations API allows you to fetch the available activations of the user.

    Task<EComServiceActivationResponse[]> GetEComActivationAsync(
    bool refresh = false,
    CancellationToken cancellationToken = default(CancellationToken));
														
#### Example: Listing the activations of the user

Refresh flag is passed to fetch the latest activations/licenses of the user. By default, refresh flag is false.

	// fetches the latest activations of the user.
	var eComServiceResponse = await TrimbleConnectEComServiceClient.GetEComActivationAsync(true);

