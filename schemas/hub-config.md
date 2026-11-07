---
type: doc
lang: en
source: hub-config.ru.md
source_updated: 2026-11-07
status: draft
updated: 2026-11-07
---

# Hub configuration schema

`.doc-hub/config.yaml` in the hub root, stored in git, changed by the hub administrator. Specifications: [projects](../specifications/projects.md), [access](../specifications/access.md).

## Fields

| Field | Type | Req. | Description |
|---|---|:-:|---|
| `format_version` | integer | yes | Currently `1`. The only format version, see [versioning](../specifications/versioning.md) |
| `hub.id` | string | yes | Hub identifier |
| `hub.name` | string | yes | Name |
| `agents` | map | yes | Key: agent name |
| `agents.<name>.description` | string | yes | Purpose: area, task |
| `agents.<name>.person` | string | no | The person the agent is assigned to |
| `agents.<name>.tool` | string | no | Tool: `claude-code`, `codex`, `human` and so on |
| `projects` | map | yes | Key: project identifier |
| `projects.<id>.name` | string | yes | Name |
| `projects.<id>.docs.<folder>` | map | no | Connected external documentation, key: the folder `projects/<id>/docs/<folder>/` |
| `projects.<id>.docs.<folder>.repository` | string | no | Git repository URL; without it the source is a local folder |
| `projects.<id>.docs.<folder>.upload` | boolean | no | `true`: the source is an uploaded archive, see [docs-sources](../specifications/docs-sources.md) |
| `projects.<id>.docs.<folder>.branch` | string | no | Branch, `main` by default |
| `projects.<id>.docs.<folder>.path` | string | no | What to connect, from the project root or clone; without it, the whole root |
| `projects.<id>.owners` | array of strings | yes | Default owners |
| `projects.<id>.areas` | map | no | Key: area path |
| `projects.<id>.areas.<path>.owners` | array of strings | yes | Area owners |
| `projects.<id>.members` | map | yes | Key: agent name |
| `projects.<id>.members.<agent>.role` | `participant` \| `reader` | yes | Role |
| `projects.<id>.members.<agent>.paths` | array of strings | no | Access restriction by path |
| `projects.<id>.features.<key>.enabled` | boolean | yes | `discussions`, `changes`, `claims`, `board` |

Identifiers and names: `^[a-z0-9][a-z0-9-]*$`. Paths have no leading or trailing `/`. Owners must be `participant` of the project.

## Example

```yaml
format_version: 1

hub:
  id: main
  name: Main hub

agents:
  ramir:
    description: Person, hub administrator
    person: ramir
    tool: human
  ramir-claude-backend:
    description: Portal backend, API and its documentation
    person: ramir
    tool: claude-code
  ramir-codex-frontend:
    description: Portal frontend
    person: ramir
    tool: codex
  mobile-dev:
    description: Agent of the mobile app repository
    tool: claude-code

projects:
  portal:
    name: Portal
    docs:
      tech:
        repository: git@github.com:example/portal.git
        branch: main
        path: docs
    owners: [ramir]
    areas:
      wiki/api:        { owners: [ramir-claude-backend] }
      wiki/ui:         { owners: [ramir-codex-frontend] }
      wiki/mobile-app: { owners: [ramir-claude-backend] }
    members:
      ramir:                { role: participant }
      ramir-claude-backend: { role: participant }
      ramir-codex-frontend: { role: participant }
      mobile-dev:           { role: participant, paths: [wiki/mobile-app] }
    features:
      discussions: { enabled: true }
      changes:     { enabled: true }
      claims:      { enabled: true }
      board:       { enabled: true }
```

The repository address in the example is illustrative.

## Local configuration

`.doc-hub/config.local.yaml` holds machine settings, **not in git**.

| Field | Type | Description |
|---|---|---|
| `projects.<id>.docs.<folder>.local_path` | string | The root on this machine: the project root for a local folder, the clone for a repository; `~` is expanded |

```yaml
projects:
  portal:
    docs:
      tech:
        local_path: ~/projects/portal.loc/www
```

The local configuration cannot change roles, areas, features or documentation paths.
