---
type: doc
lang: en
source: docs-source.ru.md
source_updated: 2026-11-06
status: draft
updated: 2026-11-07
---

# External documentation passport schema

Specification: [docs-sources](../specifications/docs-sources.md).

File `projects/<id>/docs/<folder>/.source.yaml`. Written during an export, does not reach the hub's git.

| Field | Type | Req. | Description |
|---|---|:-:|---|
| `project` | string | yes | Project identifier |
| `folder` | string | yes | Source folder name |
| `upload` | string | for an archive | Name of the uploaded file |
| `repository` | string | for a repository | Repository address |
| `branch` | string | for a repository | Source branch |
| `ref` | string | for a repository | The ref the export was made from: a branch or an explicitly given ref |
| `commit` | string | for a repository | Export commit (full SHA) |
| `path` | string | yes | What is connected, from the project root or clone |
| `paths` | array of strings | yes | Exported virtual paths; narrower than the whole folder if an agent with a restriction exported |
| `synced_at` | datetime | yes | When it was exported, ISO 8601 in UTC |
| `synced_by` | string | yes | Agent name |

```yaml
project: portal
folder: tech
repository: git@github.com:example/portal.git
branch: main
ref: main
commit: 4f2a9c1d8e7b6a5f4e3d2c1b0a9f8e7d6c5b4a39
path: docs
paths: [docs/tech]
synced_at: 2026-11-06T09:30:00Z
synced_by: ramir
```

The file name starts with a dot, so Obsidian does not show the passport.
