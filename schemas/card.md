---
type: doc
lang: en
source: card.ru.md
source_updated: 2026-11-07
status: draft
updated: 2026-11-07
---

# Card schema

Specification: [board](../specifications/features/board.md). Template: [card](../templates/card.md).

File `projects/<id>/board/<slug>.md`. If the name is taken, `-2`, `-3` and so on are added to `slug`.

## Frontmatter

| Field | Type | Req. | Description |
|---|---|:-:|---|
| `uid` | string | yes | ULID |
| `type` | `card` | yes | |
| `project` | string | yes | Project identifier |
| `title` | string | yes | What to do, in one line |
| `author` | string | yes | Who created the card |
| `assignee` | string | no | Who does the work; required for `doing` |
| `status` | `backlog` \| `todo` \| `doing` \| `done` \| `closed` | yes | Column |
| `priority` | `low` \| `normal` \| `high` | yes | Priority |
| `due` | date | no | Deadline, `YYYY-MM-DD` |
| `refs` | array of strings | no | Documents: `project:path` |
| `discussion` | string | no | File name without extension, or `uid`, of a related discussion |
| `outcome` | string | for `done`, `closed` | Outcome in 1–3 sentences |
| `created` | date-time | yes | ISO 8601 in UTC |

## Body

The task description in Markdown, then a `## History` section. The history holds one mark `[time agent → status]` per status change, with marks separated by a blank line. The section heading is the same in all languages: an implementation finds the history by it.

## Example

`projects/portal/board/refresh-token-docs.md`:

```markdown
---
uid: 01KA2Y7Q8Z3M4N5P6R7S8T9V0W
type: card
project: portal
title: Describe refresh token renewal
author: ramir
assignee: ramir-claude-backend
status: doing
priority: high
due: 2026-11-14
refs: [portal:wiki/api/auth.md]
discussion:
outcome:
created: 2026-11-07T09:00:00Z
---

Describe the token lifetime and renewal errors.

## History

[2026-11-07T09:00:00Z ramir → todo]

[2026-11-07T10:30:00Z ramir-claude-backend → doing]
```
