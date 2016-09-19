# Trimble.WebUI Release Notes

# 1.0.3
* Fixed issue with setting result, some scenarios threw exception

# 1.0.2
* Added missed method to complete the web UI flow on android
* updated the Trimble.Diagnostics dependency 1.0.10 to include the logging on UWP

# 1.0.1

* Initial code is extrated out of Trimble.Identity package, but API is refactored to make it more generic
* WebUIFactory added to instantiate WebUI for specific platform. The WebUI implementation made internal.
* PCL Profile111, net40+, Android, iOS targets supported
* WebUI for UWP is implemented
* The fragment part is supported now in end flow URL
* Code tracing is implemented with Trimble.Diagnostics