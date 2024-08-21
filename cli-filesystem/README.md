# cli-filesystem

An example of how the CLI will parse and arrange a snapshot for GitOps purposes.

Key points:

- The export will be arranged into logical directory structures, one JSON per resource for simplicity of audit.
- The syntax of references and variables is consistent with the `api-request-payload` example.  The CLI can bundle the resources together to create the import API request payload.
- The `action` on each resource is omitted, because it's up to the CLI to perform diff and the action will be different between each target environment from dev through to prod - *this may require our own state file*
- `schemaVersion` allows the import API to properly interpret and handle past and future exports formats cleanly.  Allows clients to apply version specific validation.  Allows correction on import for breaking changes to the API.