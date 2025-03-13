# Trimble Connect ECom Service Client Release Notes

# 2.1.1-beta
* Fixed the assembly version issue.
* Deprecated net40 target.
* Upgraded from MonoAndroid44 to MonoAndroid71

# 2.0.9-beta
* Updated existing document and sample reference urls with the latest trimble-oss urls.
* Updated dependencies.
* Added readme_nuget.md file.
* Added package icon file instead of package icon url.
* Added package license file instead of package license url.

# 2.0.8-beta
* Updated the `Selenium.WebDriver.ChromeDriver` module to the latest version (106.0.5249.6100).
* Updated the `Trimble.Connect.Client.Common` module to the latest version (1.0.79).
* Updated the `Trimble.Connect.Client` module to the latest version (2.6.39).
* Updated the `Trimble.Diagnostics` module to the latest version (3.0.3)

# 2.0.7-beta
* Use test automation accounts in test cases.
* Updated the `Selenium.WebDriver.ChromeDriver` module to the latest version (98.0.4758.10200).

# 2.0.6-beta
* Updated the `Selenium.WebDriver.ChromeDriver` module to the latest version (96.0.4664.4500).

# 2.0.5-beta
* Updated the `Selenium.WebDriver.ChromeDriver` module to the latest version (94.0.4606.4101).

#2.0.4-beta
* Updated the `Newtonsoft.Json` module to the (12.0.3) version for netstandard2.0 Targetframework to fix the whitesource vulnerability.
* Updated the `NUnit` module to latest version (3.13.2) to fix the whitesource vulnerability.
* Updated the `Trimble.Identity.OAuth.Password` module to the latest version (1.0.18).
* Updated the `Trimble.Connect.Client.Common` module to the latest version (1.0.65).

#2.0.3-beta
* Updated the `Trimble.Identity.OAuth.Password` module to the latest version (1.0.15) and adapted to the changes.

#2.0.2-beta
* Updated the TC logo image url for the nuget package
* Updated the `Trimble.Identity.OAuth.Password` module to the latest version (1.0.13).
* Updated the `Trimble.Connect.Client.Common` module to the latest version (1.0.57).
* Updated the `Trimble.Connect.Client` module to the latest version (2.6.16).

#2.0.1-beta
* Added a contructor to initialize TrimbleConnectEComServiceClient using ClientConfig and ICredentialsProvider. No longer supports initialization with accessToken.
* Deprecated Netstandard1.4 target and uap target (Netstandard2.0 target can be used for UWP development).
* Generating NuGet symbols package in the new (snupkg) format.

#1.0.4
* Changed white source config file to skip sonar libraries.

#1.0.3
* Made StyleCop reference private.
* Added whitesource config file.

#1.0.2
* Adapted the ECom client component to use Trimble Connect Client Common component.
* Added netstandard2.0 platform target.

#1.0.1
* Updated the response DTO for the GetActivations.

#1.0.0
* Initial version of ECom Definition service.
