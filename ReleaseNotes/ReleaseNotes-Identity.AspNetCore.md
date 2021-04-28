# Trimble.Identity.AspNetCore Release Notes

## 1.0.6-beta
* Upgraded System.Text.Encodings.Web package version to 4.5.1.
* Upgraded Microsoft.AspNetCore.Authentication.OAuth version to 1.1.3.

## 1.0.5-beta
* Added whitesource config file and fixed vulnerabilities.

## 1.0.4-beta
* Added .NETStandard2.0 build target

## 1.0.3-beta
* License text updated: https://community.trimble.com/docs/DOC-10021  

## 1.0.2-beta
* Fix version info in assembly

## 1.0.1-beta
* Initial release based on the code by Andreas Lang (<andreas_lang@trimble.com>), eCognition project
* Added validation for TID JWT tokens based on TID public key, configurable validation handlers
* Added `TrimbleAuthenticationOptions.SaveTokens` option support
* Added full JWT token parsing with the claims mapping
* Changed `TrimbleAuthenticationOptions.AuthenticationScheme` value to "Trimble"
* Changed `TrimbleAuthenticationOptions.Issuer` property value to "https://identity.trimble.com"
* Raised dependency from netstandard1.3 to netstandard1.4
* Selector for TID staging and production environments added in `TrimbleAuthenticationOptions`
* Namespace changed to `Trimble.Identity.AspNetCore`
* Developer Guide compiled with tutorial
* Sample ASP.NET Core app created