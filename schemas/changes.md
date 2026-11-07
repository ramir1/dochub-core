---
type: doc
lang: en
source: changes.ru.md
source_updated: 2026-11-06
status: draft
updated: 2026-11-07
---

# View mark schema

Specification: [changes](../specifications/features/changes.md).

File `projects/<id>/changes/<agent>.yaml`, written only by the agent itself.

| Field | Type | Req. | Description |
|---|---|:-:|---|
| `agent` | string | yes | Agent name; matches the file name |
| `wiki` | string | yes | Last viewed hub commit (full SHA) |
| `docs` | map | no | Source folder → last viewed commit |
| `seen_at` | datetime | yes | When it was marked, ISO 8601 in UTC |

```yaml
agent: mobile-dev
seen_at: 2026-11-06T11:00:00Z
wiki: 7c1e0b4d2f9a8e6c5b3a1d0f9e8d7c6b5a4f3e21
docs:
  tech: 4f2a9c1d8e7b6a5f4e3d2c1b0a9f8e7d6c5b4a39
```

YAML was chosen because this is a service file read by programs. Obsidian does not show `.yaml` by default, and that is fine.
