# Trimble.WebUI Release Notes

# 1.1.17
* Updated Xamarin.Android packages and Android target version to 7.1
* Updated UWP target version to 10.0.16299
* Updated Trimble.Diagnostics version to 2.0.13 except for uwp target.

# 1.1.11
* Added whitesource config file.

# 1.1.10
*Fix: Using WKWebView as UIWebView control is deprecated in iOS for embedded login workflow.

# 1.1.9
* Fix: Exception when embedding the UI into panel.

# 1.1.8
* Updated the nuget packages.

# 1.1.7
* Fix: Reverse changes from 1.1.5 and 1.1.6

# 1.1.6
* Fix: Exception when embedding the UI into panel if called not from UI thread

# 1.1.5
* Fix: Exception when embedding the UI into panel if called not from UI thread

# 1.1.4
* Upgrade dependency to Trimble.Diagnostics 2.0.8 that has a correct license reference

# 1.1.3
* License url has been updated in package descriptor: https://community.trimble.com/docs/DOC-10021
* Fix: delay initialization of the WebBrowser control to avoid COM exceptions

# 1.1.2
* Fix: authenticode signature recovered

# 1.1.1
* Fix the AssemblyVersion to be 1.0.0.0

# 1.1.0
* Profile111 PCL target is removed
* Migrated to Trimble.Diagnostics 2.0.6

# 1.0.10
* Improvement: LogoutAsync method is refactored for Windows desktop platforms: 
	client must call the IWebUI.ExecuteAsync explicitly when needed to reuse the standard IWebUI.ExecuteAsync for web session sign-out.
* Fix: iOS specific problem that ExecuteAsync returns before the browser window is destroyed. That creates problems with executing sequential web flows.

# 1.0.9
* Google does not allow OAuth requests to Google in embedded browsers known as “web-views”. To overcome this new limitation
  added support for SFSafariViewController on iOS and GoogleCustomTabs on Android. This support is behind the WebUIConfiguration.UseSystemBrowser option.
  The old UIWebView (iOS) and WebView (Android) preserved because it has more capabilities in some areas (customization, ending flow with http(s) url).
  SFSafariViewController is availiable on iOS 9+, on older versions library automatically falls back to UIWebView.
  If GoogleCustomTabs are not available library automatically fallback to WebView on Android.
* Improvement: automatic discovery of the parent ViewController (iOS) instead of require app to specify it in WebUIConfiguration.

# 1.0.8
* Fix (net40+): LogoutAsync can be used by console apps and from MTA same way as AcquireTokenAsync: STA created automatically if needed. 

# 1.0.7
* Fix (Android): The web view should not reload on changing orientation.

# 1.0.6
* Integrated "forgot password" flow support added: when user clicks on a "forgot password" link the page is shown in the same browser window.

# 1.0.5
* license url in the package descriptor is updated

# 1.0.4
* Added helper methods to sign-out of web session

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
