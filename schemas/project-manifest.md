---
type: doc
lang: en
source: project-manifest.ru.md
source_updated: 2026-11-06
status: draft
updated: 2026-11-07
---

# `.doc-hub.yaml` manifest schema

Specification: [project-manifest](../specifications/project-manifest.md).

## Fields

| Field | Type | Req. | Description |
|---|---|:-:|---|
| `hubs` | map | yes | Sections by hub, key: `hub.id`. The first section is the default hub |
| `hubs.<id>.path` | string | one of two | Path to the local hub clone; `~` is expanded; overridden by `DOC_HUB_PATH` |
| `hubs.<id>.url` | string | one of two | Service address; a level 3 implementation defines its meaning |
| `hubs.<id>.projects` | array of strings | yes | Hub projects that this repository is (may be empty) |
| `hubs.<id>.agent` | string | no | Default agent name, see [access](../specifications/access.md#identity) |
| `hubs.<id>.access` | array of strings | no | Other hub projects that this repository's agents work with; a hint to agents, not a grant of permissions |

The file format is YAML. The manifest holds no permissions or secrets. The file is separate on each machine and not stored in git.

## JSON Schema

The schema checks the file's contents after YAML parsing.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "DocHub project manifest",
  "type": "object",
  "required": ["hubs"],
  "properties": {
    "hubs": {
      "type": "object",
      "minProperties": 1,
      "propertyNames": { "pattern": "^[a-z0-9][a-z0-9-]*$" },
      "additionalProperties": {
        "type": "object",
        "required": ["projects"],
        "properties": {
          "path":     { "type": "string" },
          "url":      { "type": "string" },
          "projects": { "type": "array", "items": { "type": "string", "pattern": "^[a-z0-9][a-z0-9-]*$" }, "uniqueItems": true },
          "agent":    { "type": "string", "pattern": "^[a-z0-9][a-z0-9-]*$" },
          "access":   { "type": "array", "items": { "type": "string", "pattern": "^[a-z0-9][a-z0-9-]*$" }, "uniqueItems": true }
        },
        "anyOf": [ { "required": ["path"] }, { "required": ["url"] } ]
      }
    }
  },
  "additionalProperties": true
}
```
