# Trimble Connect Issue Client .NET Release Notes

## Unreleased

* Fixed incremental sync: `changeToken` is exposed on `IQueryResult<T>` for snapshot/changes endpoints.
* Issue create/update now use `IssueRequest` / `IssueUpdateRequest` (no read-only fields on write).
* Fixed `ObjectIdentity` JSON serialization (`id` on write).
* Added missing response fields: `deleted`, `index`, `_bcfImportData` on issues; `deleted` on links.
* Added permissions (GET/PATCH), reactions (GET/POST/DELETE), bulk link update, and default configuration endpoints.
* Removed non-Issues APIs: `GetVersion`, `GetCurrentUser`, and `ProjectsController`.
* `IssueClient` now shares the parent `ConnectProjectClient` HTTP stack.
* Context TRN encoding uses `Uri.EscapeDataString` via `ContextPathHelper`.

## 1.0.0

* Initial version of Issue Management service client.
* Refer to [Issue Management API Documentation](https://app.connect.trimble.com/v1/issues/swagger-ui/index.html#/)
