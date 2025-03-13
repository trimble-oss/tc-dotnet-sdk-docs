# Trimble.Diagnostics Release Notes

# 3.0.9
* Updated dependencies.
* Updated existing document and sample reference urls with the latest trimble-oss urls.
* Added readme_nuget.md file.
* Added package license file instead of package license url.

# 3.0.8
* Updated dependencies.

# 3.0.0
* Added netstandard2.0 logger - windows ewt, non windows trace
* Remove all mobile targets
* Remove obsolete Logger and SourceSwitchEx

# 2.0.14
* Removed netstandard1.1 and netstandard1.3 target frameworks to fix whitesource vulnerabilities.
* Added netstandard2.0 target framework.
* Updated the UAP target platform version to 10.0.16299.0.

# 2.0.13
* Added whitesource config file and updated nuget references.

# 2.0.12
* Updated nuget packages.

# 2.0.11
* Fix: Outputs message to attached debugger from EventLogLogger

# 2.0.10
* Added EventLogLogger outputs message to debugger also if attached

# 2.0.9
* Added EventLogLogger on Windows platforms
* Added PerformanceScope helper class

# 2.0.8
* License text updated: https://community.trimble.com/docs/DOC-10021

# 2.0.7
Fix: authenticode signature recovered

# 2.0.6
Fix: method parameters formatting in MessageFormatter.FormatMethodCall()

# 2.0.5
* Return back the xamarin.ios10, monodroid70 targets as EventSource implementation on those platform do nothing. Se we have to use a special logging implementation for those (TraceSourceLogger).

# 2.0.4
* ETW tracing (with EventSource) in netstandard1.3 is implemented

# 2.0.3
* netstandard1.1 added back with null logger for backward compartibility

# 2.0.2
* xamarin.ios10, monodroid10 and netstandard1.1 targets are replaces with netstandard1.3
* TraceSourceLogger.BeginScope() implementation does the timing for the scope now

# 2.0.1
* ILogger.BeginScope() method changed to accept a formatter instead of string

# 2.0.0

* new simplified ILogger interface
* activity logging concept with timing
* ILoggerFactory interface introduced to hide the logger implementation selection logic
* Use ETW tracing instead of TraceSource on Windows platforms (net35, net40, net45)
* Different implementations of loggers are introduced: NullLogger, EtwLogger, TraceSourceLogger
* Trace based logger implementation is removed as Xamarin now supports TraceSource

# 1.0.12

* netstandard1.1 target added instead of PCL Profile111, net35-client changed to net35, net40-client changed to net40

# 1.0.11

* license url in the package descriptor is updated

# 1.0.10

* UWP tracing is implemented with EventSource
* not used resources are removed from Android project

# 1.0.9

* net35-client target added
* net40 target changed to client profile

# 1.0.8

* Crossplatform TraceSource based logger
* PCL Profile111, .net40+, iOS, Android, UWP targets (PCL and UWP logger implementations are empty)
