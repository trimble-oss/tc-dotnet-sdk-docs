# Trimble Connect Object Sync Helper Service Client Release Notes

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
