# Trimble Connect Auth Release Notes

# 2.0.4
* License text updated: https://community.trimble.com/docs/DOC-10021 
* Upgrade dependencies so they point to packages with correct license

# 2.0.3
* Fix authenticode signing and move dependencies to properly signed assemblies. 

# 2.0.2
* No functional changes: migrated to VS2017, SDK project format. 

# 2.0.1
* The default callback url is corrected (wrong slash was used)
* The algorithm for build a startUrl for the compliance flow is extended with support for redirectTo TCW parameter
* lang parameter if exists always appended before the redirectUri parameter as required by TCW

# 2.0.0
* 'LoginOptions' and 'LoginAsync(string, LoginOptions)' are extracted to a separate Trimble.Connect.Auth component fro the Trimble.Connect.Client to decouple the WebUI dependency
* Added 'LoginOptions.Language' property
* Improvement: LoginAsync(string, LoginOptions) can discover the web flow start url from the server response automatically.
