# Trimble SQLite .NET Wrapper

# 2.0.10
* Fixed the assembly version issue.
* Deprecated net40 target.

# 2.0.9
* Fixed the OS description check for Android devices to load sqlite andriod libraries.

# 2.0.8
* Added fix for loading android libs in MAUI style android projects. The RuntimeInformation.IsOSPlatform(System.Runtime.InteropServices.OSPlatform.Linux) returns false in MAUI Android, so we additionally check the OSDescription.

# 2.0.7
# 2.0.6
# 2.0.5
* Updated Readme

# 2.0.4
* Updated sqlite version from 3.36.0 to 3.39.4.0.

# 2.0.3
* Remove old mobile targets

# 1.0.65
* Updated package icon url and symbols format to snupkg

# 1.0.64
* Updated sqlite version from 3.27.2 to 3.36.0.
* Updated the TargetPlatformVersion and TargetPlatformMinVersion of uap project to 10.0.16299.0.

# 1.0.63
* Fixed sonarqube issues and setup whitesource config file.

# 1.0.62
# 1.0.61
* Updated sqlite version from 3.14.1 to 3.27.2

# 1.0.59
* License text updated: https://community.trimble.com/docs/DOC-10021

# 1.0.58

* Renamed the embedded sqlite3 native Android library to avoid name conflict with SQLitePCLRaw package.

# 1.0.57

* Android only change: the distributed sqlite3.so library is used only on Android 7 (API24) or later

# 1.0.56

* Trimble.Diagnostics package is used for tracing

# 1.0.55

* PCL profile changed to `Profile111` to ensure future transition to `netstandard11`
* sqlite native libraries updated to 3.14.1
* Android 7 (API24) support: native sqlite3.so library is included as part of the distribution.
* UWP target added

# 1.0.54

* Assembly is strong named

# 1.0.53

* More convenience methods added to QueryBuilder
