# Trimble Connect .NET Client Common Release Notes

## [1.0.87] - 2023-07-31
* Added TopicApi Uri property in Region.cs.

## [1.0.86] - 2023-07-24
* Added file path property to file upload request.

## [1.0.85] - 2023-06-27
* Updated existing document and sample reference urls with the latest trimble-oss urls.

## [1.0.84] - 2023-06-23
* Changed the TcpsApi setter property from internal to public.

## [1.0.83] - 2023-05-30
* Added retries for http request exception while making the http calls.

## [1.0.82] - 2023-05-15
* Added retries for socket exception while making the http calls.

## [1.0.81] - 2022-11-22
* Added Regions configuration for UserApi

## [1.0.80] - 2022-11-21
* Made internals visible to UserApp Service client.

## [1.0.79] - 2022-09-29
* Updated `Trimble.Diagnostics` module to 3.0.3.
* Updated `Trimble.Net.Http.Formatting` module to 1.0.6.

## [1.0.78] - 2022-08-22
* Updated `Newtonsoft.Json` module to 13.0.1.

## [1.0.77] - 2022-07-20
* Updated the `Selenium.WebDriver.ChromeDriver` module to the latest version (103.0.5060.5300).
* Updated the `Trimble.Diagnostics` module to the latest version (3.0.3) for netstandard 2.0 target framework.

## [1.0.76] - 2022-05-12
* Improved message formatting for InvalidServiceOperationException to include all details returned by server.

## [1.0.75] - 2022-05-02
* Added support for basic authorization header in the request.
* Updated the `Selenium.WebDriver.ChromeDriver` module to the latest version (100.0.4896.6000).

## [1.0.74] - 2022-04-06
* Made internals visible to Tranos Notification client.

## [1.0.73] - 2022-03-21
* Added provision to add processing header to the request.
* Used test automation accounts in test cases.

## [1.0.72] - 2022-01-20
* Fix: Skip passing authentication header to data ocean file upload urls that are already pre-signed.

## [1.0.71] - 2021-12-10
* Adapted the UploadFileAsync method for Topics Service.

## [1.0.70] - 2021-12-10
* Made internals visible to Topics client.

## [1.0.69] - 2021-11-23
* Updated the `Selenium.WebDriver.ChromeDriver` module to the latest version (96.0.4664.4500).

## [1.0.68] - 2021-09-29
* Updated the `Selenium.WebDriver.ChromeDriver` module to the latest version (94.0.4606.4101).

## [1.0.67] - 2021-09-12
* Updated the `Trimble.Identity.OAuth.Password` module to the latest version (1.0.20) and adapted to the changes.
* Added `Selenium.WebDriver.ChromeDriver` module to the Trimble.Connect.Client.Common.Test.

## [1.0.66] - 2021-08-25
* Throw custom argument exception for invalid request uri to HttpRequestBuilder.

## [1.0.65] - 2021-08-3
* Updated the `Trimble.Diagnostics` module to (2.0.12) version.

## [1.0.64] - 2021-08-3
* Updated the `Trimble.Identity.OAuth.Password` module to the latest version (1.0.18) and adapted to the changes.

## [1.0.63] - 2021-07-21
* Updated the `Newtonsoft.Json` module to (12.0.3) version for netstandard2.0 Targetframework to fix whitesource vulnerability.
* Updated the `NUnit` module to latest version (3.13.2) to fix the whitesource vulnerability.

## [1.0.62] - 2021-06-29
* Updated the `Trimble.Identity.OAuth.Password` module to the latest version (1.0.17) and adapted to the changes.

## [1.0.61] - 2021-06-15
* Updated the `Trimble.Identity.OAuth.Password` module to the latest version (1.0.15) and adapted to the changes.

## [1.0.60] - 2021-04-26
* Removed the optional parameter passing to ClientConfig.GetServiceURIForRegion() as it is already available in ClientConfig.Environment property.

## [1.0.59] - 2021-04-25
* Throws the original server error in case of token refresh failure using ICredentialsProvider.
* The exception details while refreshing token are available using InvalidServiceOperationException.TokenRefreshException

## [1.0.58] - 2021-04-14
* Added environment property to the client config.
* Added possibility to specify the environment for which the region-specific service URIs are requested in `ClientConfig.GetServiceURIForRegion()`.

## [1.0.57] - 2021-04-13
* Updated the `Trimble.Identity.OAuth.Password` module to the latest version (1.0.13).

## [1.0.56] - 2021-04-12
* Updated the `Trimble.Identity.OAuth.Password` module to the latest version (1.0.12).

## [1.0.55] - 2021-01-28
* Updated the TC logo image url for the nuget package.

## [1.0.54] - 2020-12-16
* Fix: Skip passing authorization bearer header for pre-signed url requests.

## [1.0.53-beta] - 2020-11-11
* Made internals visible to the Trimble.Connect.BatchService.Client project.

## [1.0.52-beta] - 2020-10-21
* Updated dependency `Trimble.Identity.Oauth.Password` to version 1.0.11.
* Added a new test.

## [1.0.51-beta] - 2020-09-14
* Introduced `RegionsConfig` global configuration for keeping the project location to service regions map with possibility to initialize from the TC server or manually.
* Breaking change: Removed the dependency of `TrimbleConnectHttpClient` from regions configuration

## [1.0.50-beta] - 2020-09-14
* Fix: Made ClientConfig.RegionsUri configurable.

## [1.0.49-beta] - 2020-09-03
* Added Regions configuration to IClientConfig.
* Clients should call TrimbleConnectHttpClient.ReadConfigurationAsync() to fetch the regions configuration and use it to resolve the region end points in ClientConfig.GetServiceURIForRegion(region).

## [1.0.48-beta] - 2020-08-07
* Using portable debug symbols.

## [1.0.47-beta] - 2020-08-07
* Updated the release notes link.

## [1.0.46-beta] - 2020-08-07
* Created beta version NuGet package.

## [1.0.45] - 2020-08-06
* Deprecated NetStandard1.4 target and uap target (NetStandard 2.0 target can be used for UWP development).

## [1.0.44] - 2020-07-29
* Generating NuGet symbols package in the new (snupkg) format.

## [1.0.43] - 2020-07-06
* Excluded leftover files from SonarQube scans from the WhiteSource scans..

## [1.0.42] - 2020-07-02
* Fixed FileUploadRequest constructor that was throwing not implemented exception in netstandard 2.0
* Fixed sonarqube reported bugs.

## [1.0.41] - 2020-07-01
* Added configuration file for WhiteSource scan.
* Prevented StyleCop from propagating to consuming modules.

## [1.0.40] - 2020-06-02
* Syntax error fixed in the `ICredentialsProvider` method names

## [1.0.39] - 2020-06-01

* Added timeout and progress reporting capabilities to NDJSON streaming
* Restructured the `HttpClient` NDJSON extensions to make it more modular and testable
* Aligned the `HttpClient` extensions naming with common conventions
## [1.0.38] - 2020-05-31

* Throw `ArgumentNullException` instead of `InvalidDataException` in case of null arguments
* Allow empty uri as an argument to NDJSON helpers

## [1.0.37] - 2020-05-30
* Removed `IRequestExtensions`

## [1.0.36] - 2020-05-29
* Removed `IRequest.UseAuthorization`

## [1.0.35] - 2020-05-29
* Retry infrastructure refactored to use `DelegatingHandler` instead as the original solution was not really working (`System.InvalidOperationException`)
* `ICredentialsProvider` interface refactored to provide more information about the context to the provider, so it can authorize the request more intellegently

## [1.0.34] - 2020-05-28
* Retry infrastructure refactored

## [1.0.33] - 2020-05-26
* Added interfaces for generic model classes (**_`IModel`_**) and for request classes (**_`IRequest`_**).
* Added helper class **_`NDJsonSerializer`_** for serializing and deserializing NDJSON data.
* Added **_`HttpClientExtensions`_** extension class for **_`HttpClient`_** which currently implements sending and receiving NDJSON content.

## [1.0.32] - 2020-05-25
* Added support for automatic retrying of operations with exponential backoff and jitter and afferent tests.
* Added **_`IRetryConfig`_** interface and **_`RetryConfig`_** base class for storing configuration paramaters related to retrying requests.
* Added the client config as a member into **_`TrimbleConnectHttpClient`_** to be able to use the client configuration parameters.

## [1.0.31] - 2020-05-22
* Added `ICredentialsProvider` interface
* Added `ImmutableCredentialsProvider` as implementation of `ICredentialsProvider` interface
* Corrected the namespace for public classes to be `Trimble.Connect.Client`
* Removed `ResponseErrorCode` class, each service wrapper must define it's own servie specific error codes.

## [1.0.30] - 2020-05-20
* Encapsulate the `TrimbleConnectHttpClient` construction and initialization in the factory method. Mark constructors obsolete.
* The `ReadWriteTimeout` and `BufferSize` properties are removed from `IClientConfig` as there is no use for them at the moment.

## [1.0.29] - 2020-05-19

* `RequestId` property added to the `InvalidServiceOperationException`
* `InvalidServiceOperationException` able to capture information (error code from body and RequestId from header) from Trimble Cloud and TC new microservices as well

## [1.0.28] - 2020-05-12
* Made sure to dispose the resources after sending the request in **_`HttpRequestBuilder`_**.

## [1.0.27] - 2020-05-11
* Added support for creating object requests without the standard headers.

## [1.0.26] - 2020-05-11
* Made internals visible to the Trimble.Connect.Data.Sync project.

## [1.0.25] - 2020-05-08
* Fixed adding the timeout to the file upload api.

## [1.0.24] - 2020-05-08
* Added Upload File API 

## [1.0.23] - 2020-05-08
* Added support for accepting responses with any kind of content type.

## [1.0.22] - 2020-05-08
* Removed Accept json header to binary http request.

## [1.0.21] - 2020-05-08
* Fixed adding headers to file client.

## [1.0.20] - 2020-05-07
* Added support for accepting responses of type `NDJSON`.

## [1.0.19] - 2020-05-06
* Made the **_`TrimbleConnectHttpClient.CreateBinaryRequest()`_** methods more generic by taking any Http method as parameter.

## [1.0.18] - 2020-05-06
* Made internals visible to the Trimble connect client test project.
* Changed the namespace of **_`ResponseErrorCode`_** class so that it does not conflict with the similar class in TC Client.

## [1.0.17] - 2020-05-02
* Added file http client support in TrimbleConnectHttpClient to support file uploads and downloads.

## [1.0.16] - 2020-04-24
* Added netstandard2.0 platform target.

## [1.0.15] - 2020-04-23
* Changed default assembly file version from x.y.0.0 to x.y.9999.9999 to make debug builds distinguishable from official builds.
* Changed the user agent literal from `c-client` to `tc-client-c`

## [1.0.14] - 2020-04-21
* Fixed adding the client version user agent header by taking it from the appropriate assemblies instead of from the **_`Trimble.Connect.Client.Common`_** assembly.

## [1.0.13] - 2020-04-20
* Made internals visible to the sync helper client.

## [1.0.12] - 2020-04-10
* Added support for the **_`If-None-Match`_** HTTP header extension also in the **_`HttpRequestBuilder`_** class.

## [1.0.11] - 2020-04-08
* Added support for the **_`If-None-Match`_** HTTP header extension.

## [1.0.10] - 2020-04-03
* Removed the **_`Client`_** class and related tests.

## [1.0.9] - 2020-04-01
* Added: The **_`Client`_** class implements the **_`IDisposable`_** interface.
* Changed the accessability of the **_`Client`_** class from `public` to `internal` and the accessability to its **_`HTTPCLient`_** member to protected.
* Fixed StyleCop warnings in the test project.
* Re-organized the sorce files into subfolders for better overview.

## [1.0.8] - 2020-03-31
* Added Installation Id header to HTTP calls.

## [1.0.7] - 2020-03-30
* Added common abstaract base class for all Trimble Connect service client classes.
* Simplified the functionality of determining the service URI in the **_`ClientConfig`_** class.

## [1.0.6] - 2020-03-30
* Fixed stylecop code warnings.

## [1.0.5] - 2020-03-30
* Unified names and types related to handling URIs.

## [1.0.4] - 2020-03-27
* Added public constants (**_`NDJSON`_** content type name, HTTP **_`PATCH`_** method name).

## [1.0.3] - 2020-03-27
* Added new infrastructure classes: **_`IClient`_**, **_`IClientConfig`_**, **_`ClientConfig`_**

## [1.0.2] - 2020-03-27
* Added: new constructor for **_`HttpClient`_**, so handlers don't need to be passed as array.
* Added: allow to add user agent headers as a not parsed string

## [1.0.1] - 2020-23-23
* Fix: Nuget package name.

## [1.0.0] - 2020-23-23
* Initial version of Trimble Connect .NET client common code.
