# Trimble Connect Object Sync Helper Service Client Release Notes

# 2.1.2 
# 2.1.1
* Fixed the assembly version issue.
* Deprecated net40 target.
* Updated from MonoAndroid44 to MonoAndriod 71

#2.0.16
* Updated dependencies.
* Updated existing document and sample reference urls with the latest trimble-oss urls.
* Added readme_nuget.md file.
* Added package icon file instead of package icon url.
* Added package license file instead of package license url.

#2.0.15
* Made Triangle count property to be long type and handled nulls to return 0.

#2.0.14
* Made Triangle count property nullable to handle nulls in the response.
* Added Test case to check null handling of triangle count.

#2.0.13
* Updated Trimble.Diagnostics module to 3.0.3.
* Updated the Trimble.Connect.Client.Common module to the latest version (1.0.79).

#2.0.12
*Updated Newtonsoft.Json module to 13.0.1.
*Updated the Trimble.Connect.Client.Common module to the latest version (1.0.78).

#2.0.11
* Updated the Selenium.WebDriver.ChromeDriver module to the latest version (103.0.5060.5300).
* Updated the Trimble.Diagnostics module to the latest version (3.0.3) for netstandard 2.0 target framework.
* Updated the Trimble.Connect.Client.Common module to the latest version (1.0.77).

#2.0.10
* Use test automation accounts in test cases.
* Updated the `Selenium.WebDriver.ChromeDriver` module to the latest version (98.0.4758.10200).

#2.0.9
* Updated the `Selenium.WebDriver.ChromeDriver` module to the latest version (94.0.4606.4101).

#2.0.8
* Updated the `Newtonsoft.Json` module to (12.0.3) version for netstandard2.0 Targetframework to fix the whitesource vulnerability.
* Updated the `NUnit` module to latest version (3.13.2) to fix the whitesource vulnerability.
* Updated the `Trimble.Identity.OAuth.Password` module to the latest version (1.0.18).
* Updated the `Trimble.Connect.Client.Common` module to the latest version (1.0.65).

#2.0.7
* Updated the `Trimble.Identity.OAuth.Password` module to the latest version (1.0.17) and adapted to the changes.
* Updated the `Trimble.Connect.Client.Common` module to the latest version (1.0.62).
* Updated the `Trimble.Connect.Client` module to the latest version (2.6.19).

#2.0.6
* Updated the `Trimble.Identity.OAuth.Password` module to the latest version (1.0.15) and adapted to the changes.

#2.0.5
* Updated the `Trimble.Identity.OAuth.Password` module to the latest version (1.0.13).
* Updated the `Trimble.Connect.Client.Common` module to the latest version (1.0.57).
* Updated the `Trimble.Connect.Client` module to the latest version (2.6.16).

#2.0.4
* Updated the TC logo image url for the nuget package.

#2.0.3-beta
* Integrated 1.0.51-beta version of Trimble.Connect.Client.Common

#2.0.2-beta
* Integrated 1.0.50-beta version of Trimble.Connect.Client.Common and added support for initializing SyncHelperClient using regions.

#2.0.1-beta
* Added a contructor to initialize SyncHelperClient using ClientConfig and ICredentialsProvider. No longer supports initialization with accessToken.
* Deprecated Netstandard1.4 target and uap target (Netstandard2.0 target can be used for UWP development).
* Generating NuGet symbols package in the new (snupkg) format.

#1.0.3
* Fix: Sonarqube reported issues.
* Changed white source config file to skip sonar libraries.

#1.0.2
* Made StyleCop reference private.

#1.0.1
* Initial version of the object sync helper service.
