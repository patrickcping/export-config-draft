# sharable-package

Environment agnostic, single file download that can be safely uploaded to the library and ported between customers/partners etc.

Key points:

- Arranged the same as the `api-request-payload` example but omits the `importVariableValues` and `targetExistingResources` of the API request payload.  The client (Library, migration etc) add these fields in depending on the context.  The admin console UI, if it has an upload function, can present UX controls to ensure that variables are filled in.
- Contains `packageVersion`,`packageName` and any other metadata field to show the heritage of the package.
- `schemaVersion` allows the import API to properly interpret and handle past and future exports formats cleanly.  Allows clients to apply version specific validation.  Allows correction on import for breaking changes to the API.
- `dependencies` allows the import API and clients to ensure pre-reqs for import can be flagged up and fulfilled, such as Bill of Materals, environment type etc.