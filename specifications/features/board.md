---
type: doc
lang: en
source: board.ru.md
source_updated: 2026-11-07
status: draft
updated: 2026-11-07
---

# The "Board" feature

**Key:** `board` · **Status:** draft

It answers the question: **what needs doing and who has taken it?** The board is an optional way to manage a project's tasks. It is useful when work is shared among several agents and people. A project may also manage tasks elsewhere: Core does not require this feature.

The feature stays off until it is enabled in a project. An implementation that supports it offers it as an option.

## Model

A **card** is one file, `projects/<id>/board/<slug>.md`. There is no shared board file, so edits to different cards do not collide. A **column** is a value of the `status` field. An implementation builds the board from the card files. Schema: [schemas/card](../../schemas/card.md).

| Property | Description |
|---|---|
| `title` | What to do, in one line |
| `author` | Who created the card |
| `assignee` | Who does the work; required for `doing` |
| `status` | Column, see below |
| `priority` | `low`, `normal` or `high` |
| `due` | Deadline |
| `refs` | Documents: `project:path` |
| `discussion` | Related discussion |
| `outcome` | Outcome in 1–3 sentences; required for `done` and `closed` |

The order of cards within a column is not stored: higher priority comes first, then the earlier creation date.

## Statuses

```
backlog ──► todo ──► doing ──► done
              ▲        │
              └────────┘

from any status except done ──► closed
```

| Status | Meaning | Who sets it |
|---|---|---|
| `backlog` | An idea, not planned for work | author on creation |
| `todo` | Planned | author on creation; from `backlog`: author, project owner; from `doing`: assignee |
| `doing` | In progress | a participant, if there is no assignee or it is that participant |
| `done` | Finished, `outcome` filled in | assignee |
| `closed` | Cancelled: no longer relevant, duplicate; `outcome` filled in | author, project owner |

A **project owner** is an agent from `owners`. `done` and `closed` are final: new work gets a new card.

Taking a card means becoming its assignee and setting `doing`. Returning from `doing` to `todo` clears `assignee`, otherwise nobody else could take the card.

A status change = an edit to the frontmatter **and** a new line in the `## History` section with the mark `[time agent → status]`.

## Operations

| Operation | Who | Result |
|---|---|---|
| create | `participant` | Card file in `backlog` or `todo` with the first history line |
| edit | author, assignee, project owner | New field values and description; status and history do not change. `done` and `closed` cards do not change |
| take | see the status table | `assignee` is the agent itself, `status: doing`, history line |
| move | see the status table | Frontmatter edit and history line |
| close | see the status table | `status: closed`, `outcome`, history line |
| list, view | everyone with access | Filters: status, assignee, document, own cards |

A description cannot contain a `## History` line or a line of the form `[time agent …]`: that would allow forging the history.

## Write collisions

Different agents may change the same card, so the discussion procedure from [hub-storage](../hub-storage.md#write-collisions) applies: re-read the card, check again that the transition is allowed, and repeat the write.

## Relation to other features

| What | Where |
|---|---|
| Task documents | `refs` of the card |
| Substantive conversation | A discussion linked through `discussion` |
| Who is working right now | A [claim](claims.md): it expires, while the card stays |
| Telling the assignee | A `note` with the addressee in `to` |

## Rules for agents

General workflow: [rules/agents](../../rules/agents.md). For the board, additionally:

1. At session start, look at your unfinished cards.
2. Take a card before starting the work, not after.
3. If you cannot continue, return the card to `todo` and give the reason in the related discussion or a `note`.
4. Write the `outcome` so that it is clear without reading the card.

## Manual edits

Cards can be created and changed in Obsidian by following the schema. The Kanban view in Bases changes only `status`, see [obsidian](../../rules/obsidian.md#board).
