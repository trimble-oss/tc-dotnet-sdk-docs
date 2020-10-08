# Trimble Identity OAuth authorization code flow credentials provider

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
