# Trimble Identity OAuth authorization code flow credentials provider

## [2.1.4] - 2025-03-10
*  Updated email id and Tid user id on token refresh.

## [2.1.3] - 2025-02-02
*  Updated redirect URI handling and exposed Expires on and IdToken properties.

## [2.1.2] - 2024-12-09
*  Updated existing Trimble Identity documentation link  and added Trimble Identity OAuth AuthCode documentation reference url with the latest trimble-oss url.

## [2.1.1] - 2024-10-28
* Fixed the assembly version issue.

## [2.0.1] - 2024-06-08
* Deprecated Trimble Identity.
* Added support for MAUI (windows, mac, android and ios)
* Upgraded to net48 and netstandard2.0 and removed xamarin support
* Removed CustomCredentialsProvider
* There is breaking change on how the AuthCodeCredentialsProvider is initialized now. Use AuthContext to initialize. 

## [1.0.23] - 2023-08-10
* Updated existing document and sample reference urls with the latest trimble-oss urls.
* Added readme_nuget.md file.
* Updated dependencies.
* Added helper class to handle the parallel token refreshes.
* Added package icon file instead of package icon url.
* Added package license file instead of package license url.

## [1.0.22] - 2022-09-15
* Updated the `Trimble.Identity.OAuth.Password` module to the latest version (1.0.22).
* Updated the `Trimble.Connect.Client.Common` module to the latest version (1.0.78).

## [1.0.21] - 2021-08-26
* Added CustomTokenCredentialsProvider for login work-flows that use existing access token and refresh token.

## [1.0.20] - 2021-08-26
* Scope will be added automatically in the AuthCodeCredentialsProvider constructor.
* Updated the `Trimble.Connect.Client.Common` module to the latest version (1.0.65).

## [1.0.19] - 2020-09-27
* Updated the package icon.

## [1.0.18] - 2020-09-25
* Updated the Trimble.Identity package version to v4.0.1.

## [1.0.17] - 2020-09-25
* Updated the package tags.

## [1.0.16] - 2020-09-18
* Fixed build in VS 2019 by integrating Trimble Identity 1.2.0.
* Updated Android target to 7.1

## [1.0.15] - 2020-08-07
* Using portable debug symbols.

## [1.0.14] - 2020-07-29
* Generating NuGet symbols package in the new (snupkg) format.

## [1.0.13] - 2020-07-29
* No longer supporting .NET Standard 1.4 and uap10.0 targets (uap platform specific target deprecated as .netstandard 2.0 target should work for uap platforms from 10.0.16299 onwards).
* Added support for .NET Standard 2.0.

## [1.0.12] - 2020-07-06

* Ignore leftover files by Sonar during WhiteSource scans.

## [1.0.11] - 2020-07-06

* Updated the `Trimble.Connect.Client.Common` module to the latest version (1.0.42).

## [1.0.10] - 2020-07-03

* Added configuration file for WhiteSource scans (setting up Sonar and WhiteSource scans).

## [1.0.9] - 2020-06-09

* Added posibility to pass the `InteractiveAuthenticationRequest` parameters to the provider

## [1.0.7] - 2020-06-04

* Simple implementation of the credentials provider based on Trimble.Identity library. The token refresh approach might require more testing and thinking.

## [1.0.1] - 2020-06-04

* Skeleton project
