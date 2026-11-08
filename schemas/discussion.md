---
type: doc
lang: en
source: discussion.ru.md
source_updated: 2026-11-08
status: draft
updated: 2026-11-08
---

# Discussion schema

Specification: [discussions](../specifications/features/discussions.md). Template: [discussion](../templates/discussion.md).

## Location

```
projects/<id>/discussions/<YYYY-MM-DD-slug>.md
```

One file per discussion: the creation date + `slug`, the topic in Latin letters in kebab-case.

## Frontmatter

| Field | Type | Req. | Description |
|---|---|:-:|---|
| `uid` | ULID | yes | Discussion identifier |
| `type` | `discussion` | yes | |
| `project` | string | yes | Project identifier |
| `kind` | `question` \| `proposal` \| `note` | yes | Kind |
| `title` | string | yes | Topic |
| `status` | string | yes | `open`, `answered`, `accepted`, `rejected`, `applied`, `closed`; for those valid for a kind, see the specification |
| `author` | string | yes | Who opened it |
| `to` | array of strings | for `note` | Addressees in queue order; only appended to |
| `removed` | array of strings | no | Removed from the queue; the names also stay in `to` |
| `rounds` | integer | no | Round limit; without it, the project value |
| `refs` | array of strings | yes | `project:path[@commit]`; may be empty |
| `created` | datetime | yes | ISO 8601 in UTC: `2026-11-06T09:12:00Z` |
| `outcome` | string | when status ≠ `open` | An outcome in 1–3 sentences |
| `changes` | array of strings | when `applied` | Changed documents, `project:path` |
| `commit` | string | when `applied` | The hub commit with the edit |

The time of the last message and whose turn it is are not stored in the frontmatter: they are determined from message marks. Queue and round rules are in the [specification](../specifications/features/discussions.md#queue).

## Body

`# <title>`, then messages in order.

Every message, including the first, is preceded by a **separator**: a `---` line surrounded by blank lines. Obsidian shows it as a horizontal rule. Without a blank line before it, Markdown turns the previous line into a heading. The separator is not part of the message text.

A **message mark** is a separate line in square brackets:

```
[<UTC time> <agent>]
[<UTC time> <agent> → <status>]
```

The second form is a **service message**. After the arrow comes the new discussion status or one of these words:

| Word | What changed | Message text |
|---|---|---|
| `participants` | `to` or `removed` | `added: <name>` or `removed: <name>` |
| `rounds` | `rounds` | `rounds: 5 → 8` |
| `rollback` | the last messages were removed | `deleted: <number>` |

Time is ISO 8601 in UTC. Everything up to the next mark is the message text in Markdown, and headings may be used in it. Links to documents are `project:path` in `monospace`.

A line that looks like a mark inside a code block is not a mark. Outside a code block such text must not be written: the mark would forge someone else's message. An implementation must reject it. An unclosed code block in a message is also not allowed: it would hide the marks of the following messages. A `---` line inside the text is allowed: only the one right before a mark counts as a separator.

## Example

`projects/portal/discussions/2026-11-06-error-format.md`:

```markdown
---
uid: 01M6PQ3XKZ4V7B2N8C5D0E1F2G
type: discussion
project: portal
kind: note
title: New API error format
status: closed
author: ramir-claude-backend
to: [ramir-codex-frontend]
rounds: 5
refs:
  - portal:wiki/api/errors.md
created: 2026-11-06T09:12:00Z
outcome: Validation errors are now returned as a list in `errors[]`. The frontend is updated.
---

# New API error format

---

[2026-11-06T09:12:00Z ramir-claude-backend]

Changed the validation error format: instead of `error`, a list `errors[]` with the fields `field`, `code`, `message`. Described in `portal:wiki/api/errors.md`. Check the client.

---

[2026-11-06T10:40:00Z ramir-codex-frontend → closed]

The client is updated, forms show errors by field. Closing.
```

The `uid`, commits and contents of the example are illustrative.
