# Trimble.Identity.OAuth.AuthCode .NET SDK Developer Guide

### Content

1. [Overview](#overview)
2. [Authentication flow](#flow)
3. [Authentication with Trimble Identity](#identity)
4. [Code tracing](#tracing)
5. [Code snippets](#snippets)
   + [Example: How to acquire access token for the first time](#acquire_token)
   + [Example: How to refresh token](#refresh_token)
   + [Example: How to initialize sdk client](#initialize_client)
   + [Example: How to initialize sdk client with existing token](#initialize_client_known_token)
   + [Example: How to subscribe to token changed event](#persist_refresh_tokens)
6. [Platform specifics](#platforms)
   + [Android](#android)
   + [IOS](#ios)

### Acronyms

TID - Trimble Identity

## <a name="overview">Overview</a>

All of Trimble Connect's API endpoints are secured, and as such require users to be authenticated.

The authentication component provides the authentication context for all TC API calls.  It communicates with the _Identity_ _Provider_, handles token acquisition protocols, token refresh, and user interaction.

The main concepts in the identity component are: **AuthContext**, **AcquireToken**, **RefreshToken**.

![Sequence diagram](images/LoginSequence.png)

**AuthContext** represents a connection to the Identity Provider.  The **AuthContext** must be passed by the client application to initialize the **AuthCodeCredentialsProvider**

The **AuthCodeCredentialsProvider** has the method **AcquireTokenAsync** to obtain the access token in interactive way where it would open the web browser with TID login page for the user to log in. It returns the obtained access token.

he **AuthCodeCredentialsProvider** has the method **RefreshTokenAsync** to silently login. This is also used by the in-built retry handler in .NET SDK to retry the API calls that failed due to invalid token or expired token.

**AuthCodeCredentialsProvider** exposes the encrypted refresh token and the time the token is obtained for the applications to persist in their config files or storage locations to enable re-login and silent login workflows.

## <a name="flow">Authentication flow</a>

In a .NET client app, is is highly recommended that you use **AuthContext** to acquire a **TID access token**

**AuthContext** does the following:

1. Starts the authentication flow by redirecting the user agent to the TID authorization endpoint.  The user authenticates and consents, if consent is required.

2. The TID authorization endpoint redirects the user agent back to the **AuthContext** with an authorization code. The user agent returns an authorization code to the client application’s redirect URI.

3. The **AuthContext** requests an access token from the TID token issuance endpoint.  It then presents the authorization code to prove that the user has consented.

4. The TID token issuance endpoint returns an **access token** together with a **refresh token**.

5. The client application uses the **access token** to authenticate to the TID API.

## <a name="identity">Authentication with Trimble Identity</a>

In order to use TID authentication, the calling app must be registered with Trimble. See https://developer.trimble.com/docs/connect/getting_started.

As a result of registration, the requesting developer will receive the following OAuth2 parameters: client credentials (id and secret) and returnUrl (usually http://localhost for native apps or custom uri scheme for android/ios). These must be passed as additional parameters to acquire the token.

TID requires the tenant name in addition to the username.  The tenant name can be passed as a separate parameter to the NetworkCredential constructor or alternatively the user name can be formatted together with the tenant (UPN): `<name>@<tenant>`.

## <a name="tracing">Code tracing</a>

Tracing is implemented using [Trimble.Diagnostics](Developer%20Guide%20-%20Diagnostics.md) library.

The _Trimble.Identity_ component uses a similar approach for code instrumentation and tracing as the .NET network tracing (see [https://msdn.microsoft.com/en-us/library/ty48b824(v=vs.110).aspx](https://msdn.microsoft.com/en-us/library/ty48b824(v=vs.110).aspx)).

On desktop platforms, in order to enable tracing for **Trimble.Identity.OAuth.AuthCode** trace source, please include the following in your app.config:

    <system.diagnostics>
    <sources>
        <source name="Trimble.Identity.OAuth.AuthCode"></source>
    </sources>

    <switches>
      <add name="Trimble.Identity.OAuth.AuthCode" value="Verbose"/>
    </switches>
    </system.diagnostics>

### Code tracing on mobile platforms

On mobile platforms (iOS, Android), app.config level tracing configuration is not yet available.  To configure tracing on these platforms please use the _Trimble.Identity.Logging_ class.  It has a switch static property to configure the code tracing level based on platform.  By default, tracing is switched off (SourceLevels.Off).

    Trimble.Identity.Logging.Switch.Level = SourceLevels.Verbose;

## <a name="snippets">Code snippets</a>

### <a name="acquire_token">Example: How to acquire access token for the first time</a>

In order to acquire an initial security token, an application must present its credentials (**ClientCredentials**) as well as user credentials (**ICredentials**, **NetworkCreadential**) when calling **AcquireTokenAsync**.

The code snippet below demonstrates how to initialize the AuthenticationContext and call _AcquireTokenAsync_:

    var authCtx = new AuthContext(Config.ClientId, Config.ClientKey, Config.ClientName, Config.RedirectUrl) { AuthorityUri = new Uri(IdentityUris.StagingUri) };
    var provider = new AuthCodeCredentialsProvider(authCtx);
    var accessToken = await provider.AcquireTokenAsync();


### <a name="refresh_token">Example: How to refresh token</a>

The access token periodically expires.  The expiration time is controlled by the token issuer.  A new token can be acquired using user credentials or by refreshing the existing token using the refresh token (IAuthCodeCredentialsProvider.RefreshToken).  In the latter case, user credentials are not required.

    var newAccessToken = await provider.RefreshTokenAsync(refreshToken);

### <a name="initialize_client">Example: How to initialize sdk client</a>

Authenticate with TID and initialize the Trimble connect client. 

    var authCtx = new AuthContext(Config.ClientId, Config.ClientKey, Config.ClientName, Config.RedirectUrl) { AuthorityUri = new Uri(IdentityUris.StagingUri) };
    var provider = new AuthCodeCredentialsProvider(authCtx);
    		
    var config = new TrimbleConnectClientConfig { ServiceURI = TCServiceUri};
    config.RetryConfig = new RetryConfig { MaxErrorRetry = 1 };
    
    var client = new TrimbleConnectClient(config, provider);
    
    await client.InitializeTrimbleConnectUserAsync();

### <a name="initialize_client_known_token">Example: How to initialize sdk client with existing token</a>

Initialize the Trimble connect client with existing token.

	var authCtx = new AuthContext(Config.ClientId, Config.ClientKey, Config.ClientName, 	Config.RedirectUrl) { AuthorityUri = new Uri(IdentityUris.StagingUri) };
    var provider = new AuthCodeCredentialsProvider(authCtx, accessToken, new RefreshTokenInfo{RefreshToken = refreshToken, IsTokenEncrypted = true};
    		
    var config = new TrimbleConnectClientConfig { ServiceURI = TCServiceUri};
    config.RetryConfig = new RetryConfig { MaxErrorRetry = 1 };
    
    var client = new TrimbleConnectClient(config, provider);
    
    await client.InitializeTrimbleConnectUserAsync();

### <a name="persist_refresh_tokens">Example: How to subscribe to token changed event</a>

The client application can subscribe to the token refresh events and persist the provided encrypted refresh token in their storage for silent login workflows.

```c#
authCodeCredentialsProvider.OnTokenRefreshed += AuthCodeCredentialsProvider_OnTokenRefreshed;

private void AuthCodeCredentialsProvider_OnTokenRefreshed(string refreshToken, long timeInTicks)
        {
            //Persist the token in application storage.
        }
```

## <a name="platforms">Platform specifics</a>

### <a name="android">Android</a>

The AuthCodeCredentialsProvider must be set up with activity before calling the AcquireTokenAsync that launches the browser.

var activity = await Platform.WaitForActivityAsync();
authCodeCredentialsProvider.WithActivity(activity);

The android application should add this intent filter activity class to obtain the token.

```c#
using Android.App;
using Android.Content.PM;
using Android.OS;
using Trimble.Identity.OAuth.AuthCode;

[Activity(NoHistory = true, LaunchMode = LaunchMode.SingleTop, Exported = true)]
[IntentFilter(new[] { Android.Content.Intent.ActionView },
              Categories = new[] { Android.Content.Intent.CategoryDefault, Android.Content.Intent.CategoryBrowsable },
              DataScheme = "<CUSTOM_URI_SCHEME>")]

public class RedirectUriReceiverActivity : Activity
{
    private readonly IAuthCodeCredentialsProvider authCodeCredentialsProvider = MauiApplication.Current.Services.GetService<IAuthCodeCredentialsProvider>();
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);

        new Task(() =>
        {
            authCodeCredentialsProvider.OnReceive(this.Intent.Data.GetQueryParameter("code"));
        }).Start();

        this.Finish();
    }
}
```

If your project's Target Android version is set to Android 11 (R API 30) or higher, you must update your Android Manifest with queries that use Android's package visibility requirements. In the Platforms/Android/AndroidManifest.xml file, add the following queries/intent nodes in the manifest node:

<queries>
  <intent>
    <action android:name="android.support.customtabs.action.CustomTabsService"/>
  </intent>
</queries>


### <a name="ios">IOS</a>

The AuthCodeCredentialsProvider must be set up with viewcontroller that launches the browser before calling the AcquireTokenAsync.

	var viewController = Platform.GetCurrentUIViewController();
    authCodeCredentialsProvider.WithViewController(viewController);

In iOS, the application has to handle the redirect in AppDelegate.



```c#
    [Register("AppDelegate")]
    public class AppDelegate : MauiUIApplicationDelegate
    {
        protected override MauiApp CreateMauiApp() => MauiProgram.CreateMauiApp();

        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            IAuthCodeCredentialsProvider authCodeCredentialsProvider = IPlatformApplication.Current?.Services?.GetService<IAuthCodeCredentialsProvider>();
            new Task(() =>
            {
                authCodeCredentialsProvider.OnReceive(url.Query);
            }).Start();
            return true;
        }
    }
```

Add your app's Custom Redirect URI scheme to the Platforms/iOS/Info.plist.

```html
 <key>CFBundleURLTypes</key>
 <array>
    <dict>
        <key>CFBundleURLName</key>
        <string>App Name</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>CUSTOM_URI_SCHEME</string>
        </array>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
    </dict>
 </array>
```

	



