# uploaded-snapshots

Example snapshots uploaded from non-PingOne resources such as:

- [snapshot-pingfederate.json](snapshot-pingfederate.json) - Snapshot generated from the export of PingFederate configuration

These snapshots would be used in the promotion to use as the source to map from
a software source to a PingOne destination. (apples to oranges)

Key points:

- Created with a POST of a json configuration from software configurations (i.e. PingFederate, PingAM, ...)
- Contains `packageVersion`,`packageName`, `schemaVersion`, `dependencies` same as the sharable-package items
- Includes `snapshotSource` to specify where the snapshot originated

Snapshot Creation:

- `POST` to `{pingone api}/environments/{env-id}/snapshots` with example payload:

  ```json
  {
    "snapshotSource": {
      "type": "PING_FEDERATE_ADMIN_API",
      "version": "11.3.1.0",
      "created": "2024-08-27T18:31:07.131+00:00",
      "location": "https://pingfederate-admin-facile-migration-assistant.ping-devops.com/pf-admin-api/v1"
    },
    "snapshotResources":
      [
        {
          "action": "EXPORT",
          "resourcePath": "/idp/spConnections",
          "payload": { ... }
        }
      ]
  }
  ```

- This example above generates a snapshot simiar to that of [snapshot-pingfederate.json](snapshot-pingfederate.json)
