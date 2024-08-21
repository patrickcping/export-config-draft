# api-request-payload

The request payload for the service to import resources.

Key points:

- `snapshotResources`:
    - The snapshot resources are arranged into `snapshotResources` and contain no environment or region metadata - it is completely generic.
    - Payloads are based on the REST API request/response schema, with read-only and environment specific metadata fields removed.
    - `resourcePath` does not contain full URL because it can be assumed from the API request context.
    - `action` makes no distinction between create or update, because whatever service makes the API call provides an overwrite mapping in `targetExistingResources`, if no mapping exists then it is a create.  It's up to the client (Library, CLI, migration) to specify this ID mapping from what it itself knows of the target environment.
    - The snapshot payload itself has no environment specific metadata in it:
        - Variables and ID references are encased in handlebars to denote a field that the import API has to take care of.
        - ID references are, to the customer, arbitary string values but on export the API assumes a prefix of `__ref_` and appends the resource path and the name.
        - Variables are again arbitary values but given a prefix of `__var_`, to indicate that the import API should replace those variables with values from the variables API
        - This named approach for variables and ID references ensures consistency of export between different systems, for example multiple development (source) systems.  UUIDs will not work.
- `importVariableValues`:
    - Provides a way to inject/override variables for that import, environment variables relevant to the snapshot package.
    - It's expected that the `name` of a variable aligns with the variable name in the tenant service, but it could be arbitary to the customer and an `id` field added.  It's up to the client to specify this mapping based on what the customer provides.
    - Provides a boolean `override_platform_variable` to override what the target environment variable value is for this one import.
    - Provides a boolean `update_platform_variable` to tell the import API this should be the new variable value.
- `schemaVersion` allows the import API to properly interpret and handle past and future exports formats cleanly.  Allows clients to apply version specific validation.  Allows correction on import for breaking changes to the API.
- `dependencies` allows the import API and clients to ensure pre-reqs for import can be flagged up and fulfilled, such as Bill of Materals, environment type etc.

The API would:

- Create the relevant mappings in the target system for PUT based on `targetExistingResources`
- Fill the variable values (`{{__var_*}}` with values in the target environment `/promotionVariableDefinitions` unless there is an explicit override in the import request)
- Ensure the `dependencies` are fulfilled as pre-reqs in the environment, error if not.
- Parse the payload according to the schema definition referenced in the `schemaVersion` field
- When a resource with an `id` field with a ref value `{{__ref_*}}` is created, it goes and fills that resulting ID to the dependent configuration