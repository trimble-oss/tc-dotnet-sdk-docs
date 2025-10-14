# Trimble Connect PSet Service Client .NET Release Notes

## [2.1.3] - 2025-04-24
* Fixed mend scan vulnerability issue.

## [2.1.2] - 2024-10-24
## [2.1.1] - 2024-10-24
* Fixed the assembly version issue.
* Deprecated net40 target.

## [2.0.47] - 2024-06-20
* Updated minimum android target to API level 25 (nougat 7.1).
* Updated chrome driver to latest version.

## [2.0.46] - 2023-12-07
* Added wrapper classes, request objects and tests related to syncing PSets.

## [2.0.45] - 2023-08-09
* Updated existing document and sample reference urls with the latest trimble-oss urls.
* Updated the `Selenium.WebDriver.ChromeDriver` module to the latest version (115.0.5790.17000).
* Added readme_nuget.md file.
* Added package icon file instead of package icon url.
* Updated the `Trimble.Connect.Client.Common` module to the latest version (1.0.87).
* Added package license file instead of package license url.

## [2.0.44] - 2023-03-31
## [2.0.43] - 2023-03-29
* Update string format of IfMatch and IfNoneMatch headers.
* Updated the `Selenium.WebDriver.ChromeDriver` module to the latest version (111.0.5563.6400).

## [2.0.42] - 2022-09-27
## [2.0.41] - 2022-09-27
* Updated the `Trimble.Diagnostics` module to the latest version (3.0.3).
* Updated the `Selenium.WebDriver.ChromeDriver` module to the latest version (105.0.5195.5200).
* Updated the `Trimble.Connect.Client.Common` module to the latest version (1.0.79).

## [2.0.40] - 2022-08-17
* Updated `Newtonsoft.Json` module to 13.0.1.

## [2.0.34] - 2022-07-26
## [2.0.33] - 2022-07-22
* Updated the `Trimble.Diagnostics` module to the (3.0.3) version for netstandard 2.0 target framework.
* Updated the `Selenium.WebDriver.ChromeDriver` module to the latest version (103.0.5060.5300).
* Updated the `Trimble.Connect.Client.Common` module to the latest version (1.0.77).

## [2.0.32] - 2022-05-18
* Updated the `Trimble.Diagnostics` module to the (2.0.14) version.
* Updated the `Selenium.WebDriver.ChromeDriver` module to the latest version (101.0.4951.4100).

## [2.0.31] - 2021-03-02
* Use test automation accounts in test cases.
* Updated the `Selenium.WebDriver.ChromeDriver` module to the latest version (98.0.4758.10200).

## [2.0.30] - 2021-11-24
* Updated the `Selenium.WebDriver.ChromeDriver` module to the latest version (96.0.4664.4500).

## [2.0.29] - 2021-10-08
* Removed subscribe APIs to library, definition, pset and psetsOfLink methods and the related test cases as they are deprecated by the PSet service.
* Updated the `Selenium.WebDriver.ChromeDriver` module to the latest version (94.0.4606.4101).

## [2.0.28] - 2021-07-19
* Updated the `Newtonsoft.Json` module to the latest version (12.0.3) for netstandard2.0 Targetframework to fix the whitesource vulnerability.

## [2.0.27] - 2021-06-29
* Updated the `Trimble.Identity.OAuth.Password` module to the latest version (1.0.17) and adapted to the changes.
* Updated the `Trimble.Connect.Client.Common` module to the latest version (1.0.62) and adapted to the changes.

## [2.0.26] - 2021-06-8
* Updated the `Trimble.Identity.OAuth.Password` module to the latest version (1.0.15) and adapted to the changes.

## [2.0.25] - 2021-04-26
* Updated the `Trimble.Connect.Client.Common` module to the latest version (1.0.60) and adapted to the changes.

## [2.0.24] - 2021-04-20
* Fixed handling of the region specific service URLs in the client configuration.
* Fixed tests to work with the stage version of the TC client.
* Changed the test user account.
* Updated the `Trimble.Connect.Client.Common` module to the latest version (1.0.58).
* Updated the `Trimble.Identity.OAuth.Password` module to the latest version (1.0.13).
* Updated the `Trimble.Connect.Client.Common` module to the latest version (1.0.57).

## [2.0.23] - 2021-02-22
* Added automatic encoding of URI segments and parameters in requests. This is a _**potentially breaking change**_ for applications which have been doing the encoding on the application side.
* Updated the `Trimble.Diagnostics` dependency to the latest version (2.0.13).
* Updated the `NUnit` dependency to the latest version (3.13.1).

## [2.0.22] - 2021-02-03
* Updated the TC logo image url for the nuget package.
* Updated the `Trimble.Connect.Client.Common` dependency to the latest version (1.0.55).

## [2.0.21-beta] - 2020-10-23
* Breaking change: Removed `ListAllDefinitionsRequest`, `ListAllDefinitionVersionsRequest`, `ListAllPSetVersionsRequest` (use `ListDefinitionsRequest`, `ListDefinitionVersionsRequest`, `ListPSetVersionsRequest` instead).
* Updated the `Trimble.Connect.Client.Common` dependency to the latest version (1.0.52-beta).

## [2.0.20-beta] - 2020-10-02
* Made the tree UAC policies more restrictive in tests.
* Fixed StyleCop errors.
* Tests are now using vairable links values.

## [2.0.19-beta] - 2020-09-30
* Integrated Trimble.Connect.Client.Common v1.0.51-beta.
* Added support for initializing the PSetClient with Trimble Connect project location. RegionsConfig.InitializeFromServerAsync() should be called in this case to initialize regions.

## [2.0.18-beta] - 2020-09-03
* Fixed the package tags, cleaned up the project file and updated the dependencies of the test project.

## [2.0.17-beta] - 2020-08-07
* Using portable debug symbols.
* Updated the `Trimble.Connect.Client.Common` dependency to the latest version (1.0.48-beta).
* Support upsert semantic on tree and node operations
* Renamed `UpdateLibraryRequest`->`UpsertLibraryRequest`, `UpdateDefinitionRequest`->`UpsertDefinitionRequest`
* Renamed all properties `ID`->`Id`
* deleted `ListAllPSetsOfLinkRequest` and `ListAllPSetsRequest` structures as not needed
* redirect the `ListPSets` API `/psets?libId=&defId=` -> `/libs/<>/defs/<>/psets`
* use `ILists<>` instead of arrays everywhere
* Fix `unprocessed` property type in the changeset response

## [2.0.16-beta] - 2020-08-07
* Updated the release notes link.
* Updated the `Trimble.Connect.Client.Common` dependency to the latest version (1.0.47-beta).

## [2.0.15-beta] - 2020-07-29
* Generating NuGet symbols package in the new (snupkg) format.

## [2.0.14-beta] - 2020-07-29
* No longer supporting uap10.0 target (uap platform specific target deprecated as .netstandard 2.0 target should work for uap platforms from 10.0.16299 onwards).

## [2.0.13-beta] - 2020-07-27
* Created beta version NuGet package.

## [2.0.12] - 2020-07-27
* No longer supporting .NET Standard 1.4.

## [2.0.11] - 2020-07-06
* Updated dependencies: `Trimble.Connect.Client.Common` (1.0.43), `Trimble.Identity.OAuth.Password` (1.0.7).
* Excluded leftover files from SonarQube scans from the WhiteSource scans.

## [2.0.10] - 2020-07-02
* Fixed comparison to default value for arguments of type RequestType (fixing Sonar bugs).

## [2.0.9] - 2020-07-01
* Added configuration file for WhiteSource scan.
* Prevented StyleCop from propagating to consuming modules.
* Using the leatest version (1.0.41) of the **_`Trimble.Connect.Client.Common`_** module.

## [2.0.8] - 2020-07-01
* Added support for change sets.
* Added tests for generic requests and extended requests (forward compatibility mechanism).

## [2.0.7] - 2020-06-30
* Removed previously supported obsolete batch get request.
* Added model classes, request methods and tests related to the recommended generic batch get functionality.

## [2.0.6] - 2020-06-23
* Added model classes, request methods and tests related to PSet instances.
* Added tests for libraries, PSet definitions and PSet instances related to subscribing to notifications.

## [2.0.5] - 2020-06-09
* Added more tests related to PSet definitions.

## [2.0.4] - 2020-06-09
* Added model classes, request methods and tests related to PSet definitions.

## [2.0.3] - 2020-06-04
* Using the latest version (1.0.40) of the **_`Trimble.Connect.Client.Common`_** module.
* Support for .NET Core 2.0.
* Updated NuGet package references.
* Incorporated the latest infrastructure changes to align with the Organizer SDK.
* Re-organized source files in both the main project and the test project to follow the standardized folder structure used in other .NET SDK projects.
* Added **_`PSetModel`_** base class for model classes.
* Added **_`PSetReuqest`_** base class for request classes.
* Added model classes, request methods and tests related to libraries.
* Added model classes, request methods and tests related to library policies.
* Added model classes, request methods and tests related to miscellaneous functionality (me endpoint, subscribe to notifications).

## [2.0.2] - 2020-04-29
* Removed the Test namespace.
* Using PROD TID credentials for the tests.
* Using the latest version (1.0.16) of the **_`Trimble.Conect.Client.Common`_** module.
* Changed default assembly file version from x.y.0.0 to x.y.9999.9999 to make debug builds distinguishable from official builds.
* Changed the user agent literal for this module from `pset-client` to `tc-client-pset`.

## [2.0.1] - 2020-04-08
* Adapted the code to use the leatest version (1.0.11) of the **_`Trimble.Connect.Client.Common`_** module.
* Grouped the parameters of the methods that implement requests.
* Re-organized source files into subfolders.
* Fixed StyleCop warnings and enabled automatic StyleCop scans during build.
* Updated release notes.

## [2.0.0] - 2020-03-30
* Breaking changes to the public interface compared to previous versions (v1.0.*):
* Simplified the PSet .NET API: removed controller classes and made the major functionality accessible directly through the **_`PSetClient`_** class.
* The PSet client class has received a shorter name: **_`PSetClient`_**.
* Switched to standardized names for methods that launch requests against the PSet REST API:
* General naming convention for method prefixes is: **_`Get`_**, **_`BatchGet`_**, **_`List`_**, **_`Create`_**, **_`Update`_**, **_`Delete`_**
* For resources that don't have separate create and update methods (like policies, linksets, psets) the prefix for the create/update method is **_`Put`_**
* For asynchronous methods the **_`Async`_** suffix is used.
* In general all the data model class names and all the client method names that launch requests to the PSet service have been aligned with the Property Set API documentation.

## [1.0.2] - 2020-03-25
* Using Trimble Connect .NET Client Common module (code common to CDM services has moved into this module)
* Cleaned up references
* Fixed StyleCop warnings

## [1.0.1]
* Fix: comments.

## [1.0.0]
* Initial version of PSet Definition service.