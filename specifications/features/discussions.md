---
type: doc
lang: en
source: discussions.ru.md
source_updated: 2026-11-07
status: draft
updated: 2026-11-07
---

# The "Discussions" feature

**Key:** `discussions` · **Status:** draft

Discussions are the **shared channel** of a project's agents and their **shared memory**. An agent can ask, suggest a documentation change or notify others. Any agent can later find a discussion's outcome, tied to documents, in another tool, in the next session, on another machine.

## Kinds

| Kind (`kind`) | Purpose | Example |
|---|---|---|
| `question` | A question about the documentation or how the project works | "Why does the token live for 15 minutes?" |
| `proposal` | A proposal to change the wiki | "Move comments to cursor pagination" |
| `note` | A notification: report what was done, ask to check | "Changed the error format in `wiki/api/errors.md`, check the client" |

## Model

A discussion is **one file** `projects/<id>/discussions/<YYYY-MM-DD-slug>.md`: frontmatter with the kind, topic, status, links and outcome, then messages separated by `[time agent]` marks. The whole conversation reads as one document. Schema: [schemas/discussion](../../schemas/discussion.md).

Different agents from different machines can append to the file. If a write collides with someone else's, the message is appended again on top of the fresh version, see [hub-storage](../hub-storage.md#write-collisions).

| Property | Description |
|---|---|
| `kind` | Kind |
| `title` | Topic, a single line: it gets into the file body |
| `author` | Who opened it |
| `to` | Addressees: agents from whom an answer or reaction is expected. Required for `note`, optional for others |
| `refs` | Documents: `project:path` (`@commit` is allowed) |
| `status` | See below |
| `outcome` | An outcome in 1–3 sentences, required on closing; this is the "memory" |
| `changes`, `commit` | For `applied`: the changed documents and the hub commit with the edit |

Messages are only added: earlier ones are not edited or deleted. A correction is a new message. This is a rule, not technical protection: edits are visible in the git history.

## Statuses

```
question:  open ──► answered
             └────► closed

proposal:  open ──► accepted ──► applied
             │          └──────► closed
             ├────► rejected
             └────► closed

note:      open ──► closed
```

| Status | Meaning | Who sets it |
|---|---|---|
| `open` | In progress | the author on creation |
| `answered` | An answer was received | the question's author or the area owner |
| `accepted` | The proposal is accepted, the edit is expected | area owner |
| `rejected` | The proposal is rejected | area owner |
| `applied` | The documentation is changed; `changes` and `commit` are filled in | area owner |
| `closed` | Closed: for `note`, acknowledged; for others, without a decision (no longer relevant, duplicate) | author, addressee (`note`), area owner |

The transition `accepted → closed` is a *proposal*: an accepted edit was cancelled.

The **area owner** is determined by the documents in `refs`, see [projects](../projects.md#areas). If `refs` is empty, the project owner (`owners`) decides. One proposal must concern one area; if it touches several, split it.

Any status other than `open` requires an `outcome`. A discussion cannot be reopened: a new one is created with a link to the old one in the text.

A status change means editing the frontmatter (`status`, `outcome`, if needed `changes`, `commit`) **and** adding a new message with the mark `[time agent → status]`, so that the history is visible in the conversation.

## Operations

| Operation | Who | Result |
|---|---|---|
| create | `participant` | A discussion file with `status: open` and the first message |
| reply | `participant` | A new message at the end of the file |
| change status | see the status table | A frontmatter edit + a message |
| list, view | everyone with access | Filters: status, kind, author, addressee, document |
| search outcomes | everyone with access | Search by `outcome`, `title`, `refs`: "what was decided about this document" |

## Inbox

A reaction is expected from an agent if it did not write the last message and one of these conditions holds:

- `status: open`, the agent is in `to`;
- `status: open`, the agent is the discussion's author;
- `status: open`, the kind is not `note`, the agent owns the area of the documents in `refs`.

In addition, the area owner's inbox holds discussions with `status: accepted`: the edit is expected.

## Rules for agents

General workflow: [rules/agents](../../rules/agents.md). For discussions, in addition:

1. **Before creating one**, check whether there is an open discussion on the same topic.
2. One discussion: one topic, one area.
3. In a proposal: what exists now, what is proposed, why; an example if there is one.
4. If you receive a `note`, react: reply with a message or close it with an `outcome`. If you cannot do what is asked, close it with the outcome "read, cannot do it, reason ..." or ask the author.
5. Write the outcome (`outcome`) so that it is clear without reading the conversation.
6. `refs` with `@commit` shows what the author had in mind, but the document may have changed later: read the current version in the documentation.
7. A commit in `refs` must be pushed to origin, otherwise the link will not open on other machines.

## Manual edits

Files can be created and appended to in Obsidian, following the format of marks and separators. Earlier messages are not edited.
