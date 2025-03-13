# Trimble Connect Topic Client .NET Release Notes

## 1.2.1
* Fixed the assembly version issue.
* Deprecated net40 target.

## 1.1.6
* Added BCF 3.0 properties `server_assigned_id` and `aspect_ratio`.

## 1.1.5
* Updated dependencies.
* Updated existing document and sample reference URLs with the latest trimble-oss URLs.
* Added readme_nuget.md file.
* Added package icon file instead of package icon URL.

## 1.1.4
* Adapted to the latest viewpoint dto.

## 1.1.3
* Added support for current-user topics service API.

## 1.1.2
* Fixed type in model class for the property `viewpoint_guid`.

## 1.1.1
* Added `project_id` setter to `Identifier` in ObjectIdentity to read the updated response from topic service for get project and get projects.

# 1.1.0
* Support paging.
* Fixed namespace conflicts with TC Client.

# 1.0.8
* Fixed typo in model class for the properties `assigned_to_uuid` and `assigned_to`

# 1.0.7
* Fixed the json parsing of properties in the Model classes.

# 1.0.6
* Added an optional dictionary of user-defined parameters in the CRUD async calls in IController.

# 1.0.5
* Fixed issue with parsing related topics.

# 1.0.4
# 1.0.1
* Initial version of Topic service client.
* Refer https://github.com/buildingSMART/BCF-API 
