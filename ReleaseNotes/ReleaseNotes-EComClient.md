# Trimble Connect ECom Service Client Release Notes

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
