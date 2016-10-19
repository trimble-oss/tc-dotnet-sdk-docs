# Trimble Connect .NET SDK Developer Guide

### Content

1. [Introduction](#introduction)
2. [Components](#components)
3. [Building applications with TC SDK Components](#applications)
4. [TC environments](#environments)
5. [NuGet packages](#nuget-packages)
6. [Support](#support)


### Acronyms

TID - Trimble Identity

TC - Trimble Connect

pod - TC terminology for a regional deployment of the service

### Terminology mapping

TC .NET SDK uses different terminology comparing to TC v2 REST API in some places.

<table>
  <tr>
    <td>TC REST API</td>
    <td>TC .NET SDK Data Component</td>
  </tr>
  <tr>
    <td>Alignment</td>
    <td>Placement
(Alignment class is used for the "matrix" type of the representation of the model placement)</td>
  </tr> 
</table>


## <a name="introduction">Introduction</a>

Trimble Connect .NET SDK is: 

* a set of components, tools, examples, and guidelines, 

* for desktop, web and mobile .NET application developers (both Trimble and 3rd party) as well as for service applications (e.g. server side background and interactive processes) developers,

* built to enable collaboration functionality in applications using Trimble Connect Services or implement integration of other systems with Trimble Connect.

The Trimble Connect SDK contains all of the necessary tools and building blocks to handle user authentication and to communicate with the Trimble Connect Platform Services in order to share data and collaborate with other users and applications using the TC.

The TC .NET SDK targets full .NET desktop application (.NET 4.0 and above) as well as iOS (7.1+), Android (4.0+) and UWP applications built with Xamarin.

<!-- (Comment) Introductory presentation can be found also [TODO: insert link]().-->

API training videos are available on [youtube](http://www.youtube.com/playlist?list=PLUO6j5jr1rwtrkegAj-YNXq56Si337vPo).

### Why should I use TC .NET SDK

When creating .NET applications developers can choose different approaches to communicate with TC Services. They can opt to rely on the TC REST API directly and utilize built-in HttpWebRequest or HttpClient classes available in the .NET platform or use generic 3d party libraries like RestSharp (http://restsharp.org/) that help to communicate with REST services.

Using these generic network .NET APIs and libraries gives application developer full flexibility in how to communicate with the TC Services and how to organize the app code, but it might be not the most productive approach comparing to the specialized SDK. 

In addition to TC API usage the TC SDK boosts developer productivity by providing reusable ready to use functional blocks that are typically needed in real life apps. This helps building high quality applications by reusing already implemented and tested components.

The TC .NET SDK is used by the core TC applications (TCD, TCM), as well as multiple other Trimble products integrating with the TC. That proves the usability of the SDK.

All SDK components are cross platform. The same API is available on a number of target platforms. This allows to share code between mobile and desktop applications and significantly reduce the development time for a family of applications.

Below are benefits listed for each component in the TC .NET SDK.

#### Trimble.Identity

User identification component ([Trimble.Identity](Developer%20Guide%20-%20Identity.md)) solves the challenge of authenticating the user with the Trimble Identity service. Implementing this functionality is proven to be challenging since it includes a user interaction with the web UI flow. This component abstracts all the complexity of the authentication from the developer behind a very simple interface yet providing a significant degree of flexibility.

#### Trimble.Connect.Client

TC API Wrappers component ([Trimble.Connect.Client](Developer%20Guide%20-%20Client.md)) can be seen as a layer on top of the generic network library. Below are some benefits of using the TC API Wrappers component in comparison to consuming the TC REST API with generic .NET libraries:

* The SDK components’ APIs are optimized to be used with specific TC Services in comparison to the generic APIs.
* Strong typing helps productivity by providing compile time error checking
* Fully built-in IntelliSense documentation support enables learning the TC API by exploring it from the IDE
* Comes with production ready components needed for the interaction with the cloud service (http message formatting and parsing, error handling, token management and caching, pods connection management) - developers can start working on features instead of building infrastructure.
* The common challenge for the connected applications is aligning the service API versioning and application versioning. The service communication code in SDK is designed and implemented with extensibility in mind and provide a set of extensibility mechanisms which help application developers to build backward and forward compatible applications.

#### Trimble.Connect.Data

One significant addition in the SDK over the direct REST API usage is the local (offline) storage component([Trimble.Connect.Data](Developer%20Guide%20-%20Data.md)) with synchronization capabilities. This component enables building occasionally connected applications. This might be a typical challenge on a construction site where good network connection might be not available. This means:

* Application's user perceived performance is not affected by network quality
* End user can continue his/her work regardless of the network or backend availability

## <a name="components">Components</a>

TC .NET SDK is provided as a set of components.

![SDK components](images/sdk_components.png)

TC SDK components are implemented as following nuget packages:

1. [Trimble.Identity](Developer%20Guide%20-%20Identity.md) - TID authentication

2. [Trimble.Connect.Client](Developer%20Guide%20-%20Client.md) - TC API Client (TC REST API wrappers)

3. [Trimble.Connect.Data](Developer%20Guide%20-%20Data.md) and Trimble.Connect.Data.Sync - TC Data Storage (local offline storage) which uses SQLite for storing data locally with synchronization capability

All nuget packages target full .NET as well as PCL, UWP, iOS, and Android Xamarin platforms.

The [NuGet packages](#nuget-packages) are available from the [nuget.org](https://www.nuget.org/) and the corresponding symbols from symbolsource.org.  

## <a name="applications">Building Applications with TC SDK Components</a>

### Application design guidelines

This section shows typical application designs using the TC SDK components.

The design of offline components is based on the idea that applications (which want to work in occasionally connected mode) always use same interface to manage data regardless whether a network connection is currently available or not.

The local offline storage component exposes an _ISync_ interface which allows occasional (application driven) synchronization of local data with a the TC cloud backend.

![Occasionally connected application design](images/application_design.png)

Below are three typical app architecture examples:

1. Use SDK components for local storage and synchronizing with TC cloud backend.

2. Build an app with its own local storage. Using TC cloud backend and the custom local storage.

3. Application without local state (online only app). Using TC cloud backend without local storage.

![Offline capable app using SDK local storage](images/offline_capable_app_with_sdk_ls.png)![Offline capable app using own local storage](images/offline_capable_app_with_custom_ls.png)

![Online only](images/online_only_app.png)

### Configuring Visual Studio

Since the TC data model uses the _ToDo_ term for one of the entity types this could conflict with “TODO:” comments that are typically used by developers and recognized by the IDE as a keyword. To avoid conflicts, the following settings are recommended in Visual Studio when using TC .NET SDK:

1. Configure ReSharper (if you are using it) to honor the case-sensitivity when searching for "TODO"

2. Add "todo" and "todos" as RecognizedWords to StyleCop settings (again, if you are using StyleCop).

## <a name="environments">TC Environments</a>

TC has several deployment environments that we encourage you to use for debugging, testing, and production.

1. Staging - this environment is recommended for integrators to use during development of connected applications:

    * Staging TC v2 API: [https://app.stage.connect.trimble.com/tc/api/2.0/](https://app.stage.connect.trimble.com/tc/api/2.0/) 

    * Staging TCW: [https://app.stage.connect.trimble.com/tc/app/](https://app.stage.connect.trimble.com/tc/app/) 

2. Production - production Trimble Connect environment

    * Production TC v2 API: [https://app.connect.com/tc/api/2.0/](https://app.connect.trimble.com/tc/api/2.0/) 

    * Production TCW: [http://connect.trimble.com/](http://connect.trimble.com/) 

The staging environment of TC uses staging environment of TID.  Production TC environment uses the production TID environment.

TID environments

* Staging TID: [https://identity-stg.trimble.com/i/oauth2/](https://identity-stg.trimble.com/i/oauth2/) 

* Production TID: [https://identity.trimble.com/i/oauth2/](https://identity.trimble.com/i/oauth2/) 

## <a name="nuget-packages">NuGet packages</a>

You can find the official releases on the public stable channel: [https://www.nuget.org/profiles/TrimbleConnect](https://www.nuget.org/profiles/TrimbleConnect).
The beta channel is also available for restricted audience: [http://ts-nuget.teklaad.tekla.com/nuget/tempfortcd/](http://ts-nuget.teklaad.tekla.com/nuget/tempfortcd/)

All published packages support the following platforms:

* .NET 4.0+
* Android 4.0+ (using Xamarin Platform)
* iOS 7.1+ (using Xamarin Platform)
* UWP

The corresponding symbol packages for stable channel can be found at [http://srv.symbolsource.org/pdb/Public](http://srv.symbolsource.org/pdb/Public).
For the beta channel symbol packages are available as well from this source: [http://ts-nuget.teklaad.tekla.com/symbols/tempfortcd](http://ts-nuget.teklaad.tekla.com/symbols/tempfortcd).

SymbolSource has good instructions on how package consumers can [configure Visual Studio to use the symbol packages ](http://www.symbolsource.org/Public/Home/VisualStudio).

<!--## <a name="samples">Sample apps</a>

Example apps can be found here [TODO: insert link]().-->

## <a name="support">Support</a>

Send email to [connect-integrate@trimble.com](mailto:connect-integrate@trimble.com)

