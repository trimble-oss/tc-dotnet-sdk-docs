# Trimble Connect Org Service Client .NET Release Notes

## [1.0.48] - 2021-04-26
* Updated the `Trimble.Connect.Client.Common` module to the latest version (1.0.60) and adapted to the changes.

## [1.0.47] - 2021-04-20
* Fixed handling of region specific service URLs in the client configuration.
* Fixed tests to work with the stage version of the TC client.
* Changed the test user account.
* Updated the `Trimble.Connect.Client.Common` module to the latest version (1.0.58).
* Updated the `Trimble.Identity.OAuth.Password` module to the latest version (1.0.13).
* Updated the `Trimble.Connect.Client.Common` module to the latest version (1.0.57).

## [1.0.46] - 2021-03-05
* Added support for node meta-data.

## [1.0.45] - 2021-02-15
* Added automatic encoding of URI segments and parameters in requests. This is a _**potentially breaking change**_ for applications which have been doing the encoding on the application side.
* Updated the `Trimble.Diagnostics` dependency to the latest version (2.0.13).
* Updated the `NUnit` dependency to the latest version (3.13.1).

## [1.0.44] - 2021-02-03
* Updated the TC logo image url for the nuget package.
* Updated the `Trimble.Connect.Client.Common` dependency to the latest version (1.0.55).

## [1.0.43-beta] - 2020-10-23
* Added support for searching nodes in trees and forests.
* Breaking change: Removed `ListAllTreesRequest`, `ListAllNodesRequest`, `ListAllNodeVersionsRequest` (use `ListTreesRequest`, `ListNodesRequest`, `ListNodeVersionsRequest` instead).
* Updated the `Trimble.Connect.Client.Common` dependency to the latest version (1.0.52-beta).

## [1.0.42-beta] - 2020-10-06
* Added support for initializing the OrgClient with Trimble Connect project location. RegionsConfig.InitializeFromServerAsync() should be called in this case to initialize regions.

## [1.0.41-beta] - 2020-10-02
* Made the tree UAC policies more restrictive in tests.
* Updated the `Trimble.Connect.Client.Common` dependency to the latest version (1.0.51-beta).

## [1.0.40-beta] - 2020-10-01
* Fixed potential null reference problem related to handling node geometries.
* Updated the `Trimble.Connect.Client.Common` dependency to the latest version (1.0.50-beta).

## [1.0.39-beta] - 2020-09-02
* Fixed project file and updated the dependencies of the test project.

## [1.0.38-beta] - 2020-08-28
* Added DeleteNodeGeometry operation

## [1.0.36-beta] - 2020-08-27
* Support deep insert nodes as part of the tree creation API
* Support upsert semantic on tree and node operations
* Removed changeset API endpoints that support tree upsert semantic added in `1.0.35-beta`
* Renamed `UpdateTreeRequest`->`UpsertTreeRequest`, `UpdateNodeRequest`->`UpsertNodeRequest`, `UpdateTreeWithChangeSetRequest`->`UpsertTreeModel`
* Renamed all properties `ID`->`Id`

## [1.0.35-beta] - 2020-08-11
* All changeset API wrappers now use new `scope` query parameter instead of passing parameters in the request body (breaking change)
* Added support for tree operations in the changeset API
* Added new changeset API endpoints that support tree upsert semantic

## [1.0.34-beta] - 2020-08-07
* Using portable debug symbols.
* Updated the `Trimble.Connect.Client.Common` dependency to the latest version (1.0.48-beta).

## [1.0.33-beta] - 2020-08-07
* Updated the release notes link.
* Updated the `Trimble.Connect.Client.Common` dependency to the latest version (1.0.47-beta).

## [1.0.32-beta] - 2020-07-31
* Added support for node geometries.
* Fix for the concurrency support on UpdateNode operation.
* Updated the dependencies.

## [1.0.31-beta] - 2020-07-29
* Generating NuGet symbols package in the new (snupkg) format.

## [1.0.30-beta] - 2020-07-29
* No longer supporting uap10.0 target (uap platform specific target deprecated as .netstandard 2.0 target should work for uap platforms from 10.0.16299 onwards).

## [1.0.29-beta] - 2020-07-27
* Created beta version NuGet package.

## [1.0.28-beta] - 2020-07-27
* Created beta version NuGet package.

## [1.0.27] - 2020-07-27
* No longer supporting .NET Standard 1.4.

## [1.0.26] - 2020-07-08
* Renamed the test methods related to generic batch-get functionality.

## [1.0.25] - 2020-07-06
* Updated dependencies: `Trimble.Connect.Client.Common` (1.0.43), `Trimble.Identity.OAuth.Password` (1.0.7).
* Excluded leftover files from SonarQube scans from the WhiteSource scans.

## [1.0.24] - 2020-07-02
* Fixed comparison to default value for arguments of type RequestType (fixing Sonar bugs).

## [1.0.23] - 2020-07-01
* Added configuration file for WhiteSource scan.
* Prevented StyleCop from propagating to consuming modules.
* Using the leatest version (1.0.41) of the **_`Trimble.Connect.Client.Common`_** module.

## [1.0.22] - 2020-06-26
* Fixed handling unprocessed nodes in change sets.

## [1.0.21] - 2020-06-25
* Updated release notes.

## [1.0.20] - 2020-06-24
* Batch-get related small fixes.

## [1.0.19] - 2020-06-11
* Replaced usages of resource arrays with `IList<Resource>` for more convenient utilization.

## [1.0.18] - 2020-06-09
* Minor improvements (eg. using `IList<string>` instead of string arrays where applicable).

## [1.0.17] - 2020-06-04
* Take in use the Trimble Identity OAuth providers in the test code

## [1.0.16] - 2020-06-03
* Minor cleanups and refactorings
* Fixed null value handling for UserClaims.UAT.

## [1.0.15] - 2020-06-02
* Token management infrastructure taken in use with `ICredentialsProvider`
* Naming for Subscription API simplified
* `IRequest.UseAuthorization` parameter removed
* dependency on `Trimble.Identity` removed in tests
* Fixed the Linux/OSX compartibility
* netstandard2.0 target added to align with the `Trimble.Connect.Client.Common` targets
* Fixed exception thown on null arguments (`InvalidDataException` -> `ArgumentNullException`)

## [1.0.14] - 2020-05-27
* Using the latest version (1.0.33) of the **_`Trimble.Connect.Client.Common`_** module.

## [1.0.13] - 2020-05-25
* Using the leatest version (1.0.32) of the **_`Trimble.Connect.Client.Common`_** module.
* Refactored the request model classes (added more properties for more flexibility).
* Refactored the request sending mechanism, now also using the binary file client amongside the object client for large content.
* Exposed generic request sending mechinism through the public interface of the client class, making it possible to generically support future request type without modifying the .NET SDK.
* Added model classes, request methods and tests related to batch-get functionality.
* Added model classes, request methods and tests related to change set functionality.
* Added utility classes for sending and receiving, serializing and deserializing NDJSON content.
* Moved the validation from **_`IModel`_**/**_`OrgModel`_** to **_`IRequest`_**/**_`OrgRequest`_** because validation is only needed for request model classes.
* Simplified and consolidated the validation of all request model classes.
* Refactored the creation of baseline test data and made it more robust.
* Improved documentation comments
* Re-organized the model classes into subfolders.
* Simplified the names of some model classes.
* Added support for generic extra properties in request classes (ability to send to the service properties which are not yet supported in the .NET SDK)(forward compatibility).
* Added support for generic extra properties in model classes (ability to receive from the service properties which are not yet supported in the .NET SDK)(forward compatibility).
* Added support for extra query parameters in request classes (which are not yet supported by the .NET SDK)(forward compatibility).
* Added support for sending generic requests to the service (which are not yet supported in the .NET SDK)(forward compatibility).

## [1.0.12] - 2020-04-29
* Using the leatest version (1.0.16) of the **_`Trimble.Connect.Client.Common`_** module.
* Added model classes, request methods and tests related to tree policies.
* Added model classes, request methods and tests related to nodes, node links and node versions.
* Added model classes, request methods and tests related to subscribing to notifications.
* Added more test related to trees and nodes.
* Made sure that all properties of request model classes which are not meant to be serialized will be ignored during serialization.
* Simplified the inheritance hierarchy of requests (removed abstract base class **_`Request`_**).
* Separated generic data model related fuctionality out of the request interface and base class (**_`IRequest`_**, **_`OrgRequest`_**) into its own interface and base class (**_`IModel`_**, **_`OrgModel`_**).
* Implemented validation for all model classes.
* Re-organized source files into subfolders.
* Using PROD TID URI and a different user account for the unit tests.
* Cleaned up the settings files.
* Changed default assembly file version from x.y.0.0 to x.y.9999.9999 to make debug builds distinguishable from official builds.
* Changed the user agent literal for this module from `org-client` to `tc-client-org`.

## [1.0.11] - 2020-04-21
* Using the leatest version (1.0.14) of the **_`Trimble.Connect.Client.Common`_** module.
* Added model classes, request methods and tests related to trees.
* Moved the request-specific details from the request methods into the request model classes and created unified request dispatching mechanism.
* Implemented unified basic request validation mechanism.
* Re-organized source files into subfolders.

## [1.0.10] - 2020-04-08

* Adapted the code to use the leatest version (1.0.11) of the **_`Trimble.Connect.Client.Common`_** module.
* Re-organized the **_`OrgClient`_** class by moving the request implementations into the **_`OrgClient.Requests`_** partial class.
* Simplified the inheritance hierarchy of the test classes.
* Aligned the model classes with the [Org service REST API documentation](https://app.swaggerhub.com/apis/Trimble-Connect/org-prod/v1#/)
* Re-organized source files into subfolders.
* Cleaned up and updated package references.
* Fixed StyleCop warnings and enabled automatic StyleCop scan during build.
* Updated release notes to use a more standardized format.

## [1.0.9] - 2020-03-27

* Code refactored to build a skeleton with design patterns to follow (naming, **_`IClient`_**, **_`IClientConfig`_**, no controller layer).

## [1.0.8] - 2020-03-26

* Reverse **_`MSBuild.Sdk.Extras`_** dependency updated.

## [1.0.7] - 2020-03-26

* **_`MSBuild.Sdk.Extras`_** dependency updated.

## [1.0.6] - 2020-03-26

* Using Trimble Connect .NET Client Common module (code common to CDM services has moved into this module).

## [1.0.5] - 2020-03-13

## [1.0.4] - 2020-03-10

## [1.0.3] - 2020-03-10

## 1.0.0

* Initial skeleton project. Empty interfaces and tests. Might require restructure still.
