---
type: doc
lang: en
source: claim.ru.md
source_updated: 2026-11-06
status: draft
updated: 2026-11-07
---

# Claim schema

Specification: [claims](../specifications/features/claims.md). Template: [claim](../templates/claim.md).

File `projects/<id>/claims/<agent>-<slug>.md`, written only by the agent itself; any member may delete an expired claim.

## Frontmatter

| Field | Type | Req. | Description |
|---|---|:-:|---|
| `type` | `claim` | yes | |
| `project` | string | yes | Project identifier |
| `agent` | string | yes | Who is working; matches the start of the file name |
| `paths` | array of strings | yes | Virtual documentation paths; optionally, code paths from the project repository root |
| `topic` | string | yes | What is being done |
| `created` | datetime | yes | ISO 8601 in UTC |
| `expires` | datetime | yes | ISO 8601 in UTC |
| `discussion` | string | no | File name without the extension or `uid` of a linked discussion |

The body is optional: a work plan in a few lines.

## Example

`projects/portal/claims/ramir-claude-backend-auth-docs.md`:

```markdown
---
type: claim
project: portal
agent: ramir-claude-backend
paths: [wiki/api/auth.md, wiki/mobile-app/03-authorization.md]
topic: Describing refresh token renewal
created: 2026-11-06T09:00:00Z
expires: 2026-11-06T13:00:00Z
---
```

Markdown was chosen so that claims are visible to people in Obsidian.
