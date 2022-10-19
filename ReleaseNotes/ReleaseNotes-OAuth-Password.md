# Trimble Identity OAuth Password Grant flow credentials provider

## [1.0.23] - 2022-10-02
* Updated nuspec.

## [1.0.22] - 2022-09-14
* Upgraded the `Newtonsoft.Json` version to 13.0.1.
* Upgraded the `Trimble.Connect.Client.Common` version to 1.0.78.
* Upgraded the `Selenium.WebDriver.ChromeDriver` version to 105.0.5195.5200 to match the latest chrome version.

## [1.0.21] - 2022-02-18
* Use test automation accounts for testing. 
* Adapted the authenticator to test automation login.

## [1.0.20] - 2021-09-9
* Moved the `Selenium.WebDriver.ChromeDriver` module from Trimble.Identity.OAuthPassword to Trimble.Identity.OAuth.Password.Test .
* Removed `Selenium.WebDriver.ChromeDriver` module from nuspec file.

## [1.0.19] - 2021-09-2
* Updated the nuspec file.
* Updated the `Trimble.Connect.Client.Common` module to the latest version (1.0.65).

## [1.0.18] - 2021-08-2
* Upgraded the `Selenium.WebDriver.ChromeDriver` version to 92.0.4515.107 to match the latest chrome version.

## [1.0.17] - 2021-06-14
* Removed Trimble.Identity.OAuth package reference.

## [1.0.16] - 2021-06-10
* Added TidUser property in PasswordCredentialsProvider class to fetch the user details.

## [1.0.15] - 2021-05-19
* Created nuspec to copy the dependency exe and dlls to the output directory.

## [1.0.14] - 2021-05-12
* Using chrome web driver for password grant workflow as the password grant is disabled in TID v4.
* Removed net40, android and ios targets as this package is supposed to be used only for running sdk test cases.

## [1.0.13] - 2021-04-13
* Updated the TC logo image url for the nuget package.
* Updated the `Trimble.Identity.OAuth` module to the latest version (1.0.16).

## [1.0.12] - 2021-04-12
* Updated the `Trimble.Identity.OAuth` module to the latest version (1.0.15).

## [1.0.11] - 2020-09-02
* Updated the package tags and `Trimble.Identity.OAuth` module to the latest version (1.0.14).

## [1.0.10] - 2020-08-10
* Using portable debug symbols.

## [1.0.9] - 2020-07-29
* Generating NuGet symbols package in the new (snupkg) format.

## [1.0.8] - 2020-07-29
* No longer supporting .NET Standard 1.4 and uap10.0 targets (uap platform specific target deprecated as .netstandard 2.0 target should work for uap platforms from 10.0.16299 onwards).

## [1.0.7] - 2020-07-06

* Updated the `Trimble.Identity.OAuth` module to the latest version (1.0.9).
* Added rule to ignore leftover files by Sonar during WhiteSource scans.

## [1.0.6] - 2020-07-06

* Updated the `Trimble.Connect.Client.Common` module to the latest version (1.0.42).
* Updated the `Trimble.Identity.OAuth` module to the latest version (1.0.7).

## [1.0.5] - 2020-07-03

* Added configuration file for WhiteSource scans (setting up Sonar and WhiteSource scans).

## [1.0.4] - 2020-06-05

* cleanup dependencies

## [1.0.3] - 2020-06-04

* password credentials provider implementation

## [1.0.1] - 2020-06-03

* Skeleton project
