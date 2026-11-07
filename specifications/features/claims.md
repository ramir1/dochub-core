---
type: doc
lang: en
source: claims.ru.md
source_updated: 2026-11-06
status: draft
updated: 2026-11-07
---

# The "Claims" feature

**Key:** `claims` · **Status:** draft

Answers the question: **who is working on this right now?** It is needed when several agents work in parallel (sessions by area, different tools, different machines), so that they do not take on the same thing.

A claim is **a warning, not a lock**: the hub cannot forbid editing a repository.

## Model

A claim is a file `projects/<id>/claims/<agent>-<slug>.md`, written only by the agent itself. Schema: [schemas/claim](../../schemas/claim.md).

| Property | Description |
|---|---|
| `agent` | Who is working |
| `paths` | Virtual documentation paths (`wiki/…`, `docs/<folder>/…`) and, optionally, code paths from the project repository root |
| `topic` | What is being done, in one line |
| `created` | When it was set |
| `expires` | When it expires |
| `discussion` | Optional: a linked discussion |

A claim is **active** while the file exists and `expires` has not passed. Expired claims are ignored; any member may delete them (an exception to the "one file, one author" rule).

## Operations

| Operation | Who | What it does |
|---|---|---|
| set | `participant` | Creates the file. Check overlaps first |
| extend | author | Changes `expires` |
| release | author | Deletes the file (the history stays in git) |
| list | everyone with access | Active claims, filter by paths |
| check overlap | everyone | Whether other agents have active claims on paths overlapping the given ones |

Paths **overlap** if one is a prefix of the other (`wiki/api` and `wiki/api/auth.md`).

## Rules for agents

General workflow: [rules/agents](../../rules/agents.md). For claims, in addition:

1. A claim is needed for an edit that takes noticeable time or runs in parallel with other agents. Other agents will not have time to see a one-step claim.
2. If you find someone else's active claim on the same paths, do not start in parallel: write to its author (`note` or `question`) or pick another task.
3. A claim is not a place for discussion: everything of substance goes in discussions.
