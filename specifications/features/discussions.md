---
type: doc
lang: en
source: discussions.ru.md
source_updated: 2026-11-08
status: draft
updated: 2026-11-08
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
| `to` | Addressees: agents from whom an answer or reaction is expected, in queue order. Required for `note`, optional for others. Only appended to |
| `removed` | Removed from the queue, see [participants](#participants) |
| `rounds` | Round limit, see [rounds](#rounds) |
| `refs` | Documents: `project:path` (`@commit` is allowed) |
| `status` | See below |
| `outcome` | An outcome in 1–3 sentences, required on closing; this is the "memory" |
| `changes`, `commit` | For `applied`: the changed documents and the hub commit with the edit |

Messages are only added: earlier ones are not edited or deleted. A correction is a new message. The exception is a [rollback](#rollback) of the last messages. This is a rule, not technical protection: edits are visible in the git history.

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

## Queue

Participants write in a discussion in turn: whoever starts first must not answer for everyone.

The **queue** is the author, then `to` in the order written, except those in `removed` and except readers (`reader`). A discussion with only the author is an ordinary discussion with a queue of one participant.

- **The turn** belongs to the one after whoever last wrote a participant message in the queue. The order is taken from the list "author, then `to`" including removed names: this way the turn moves on even if the last writer was removed. After the last one in the list the turn returns to the author.
- **Service messages** do not count in the queue: status changes and the marks `→ participants`, `→ rounds`, `→ rollback`.
- **A side remark** is a message from a person who is not in the list "author, then `to`". A person is an agent with `tool: human` in the hub configuration; for a name with a suffix, by the base agent. A side remark can be written at any time, except in a used-up discussion. It does not move the queue and does not count in rounds.
- **Nobody else may write:** an agent out of turn, a removed participant, an agent not in the list.
- **A status change does not wait for the queue:** it is made by whoever is allowed to by the status table.

The queue and the round limit are checked by the implementation. At level 1 they rest on the rules, see [levels](../../concepts/levels.md).

## Rounds

A **round** is a full pass of the queue: the participants from `to` answer and the author closes the round. Completed rounds equal the number of participant messages the author wrote after the first one. In a discussion with only the author, a round is each further message of the author.

`rounds` is the round limit. It is written on creation: by default `features.discussions.rounds` of the project, or 5 if that is not set. The author may set less, but not more. If the file has no `rounds`, the project value applies. Participant messages are then at most `1 + rounds × the number of queue participants`.

- **The last round:** `rounds − 1` rounds are completed, and each queue participant has one message left. The implementation warns about it: time to sum up.
- **Used up:** `rounds` rounds are completed. Nobody may write, side remarks included. A status change with an outcome is possible, or continuing in a new record (a discussion, the wiki, a task) with a link to this discussion.
- **The limit can be raised** by the area owner, never lowered. Raising means editing `rounds` and adding the message `[time agent → rounds]` with text like `rounds: 5 → 8`.

## Participants

Changing participants is an optional operation: an implementation may not support it, then the participants are set on creation. The format is the same for everyone.

- **Add:** the name is appended to the end of `to`. If it was in `removed`, it is taken out of there and the participant returns to the old place.
- **Remove:** the name is added to `removed`. It is not struck out of `to`, so that the order is kept. The author cannot be removed.
- **Who:** the author and the area owner. Only a project member with the `participant` role can be added.
- A change means editing the frontmatter and adding the message `[time agent → participants]` with the text `added: <name>` or `removed: <name>`.

A typical case: an agent is unavailable and the whole queue waits for it. Then the author or the area owner removes it and the discussion goes on.

## Rollback

A rollback removes the last messages from the end of the file, for example an idle round.

- Only participant messages and side remarks are removed. A rollback does not remove a service message and does not go past it. The first message cannot be removed.
- **Who:** the author and the area owner.
- A rollback means removing the messages and adding the message `[time agent → rollback]` with the text `deleted: <number>`. The removed text stays in the hub's git history.
- The turn and the rounds are recomputed from the file: after a rollback the turn returns to whoever would write next.

## Operations

| Operation | Who | Result |
|---|---|---|
| create | `participant` | A discussion file with `status: open`, `rounds` and the first message |
| reply | a queue participant on their turn; a person outside the list as a side remark | A new message at the end of the file |
| change status | see the status table | A frontmatter edit + a message |
| change participants | author, area owner | An edit of `to` or `removed` + a service message; the operation is optional |
| raise the round limit | area owner | An edit of `rounds` + a service message |
| roll back | author, area owner | Removal of the last messages + a service message |
| list, view | everyone with access | Filters: status, kind, author, addressee, document |
| search outcomes | everyone with access | Search by `outcome`, `title`, `refs`: "what was decided about this document" |

## Inbox

A reaction is expected from an agent if `status: open` and one of these conditions holds:

- it is the agent's turn and the discussion is not used up;
- the discussion is used up and the agent is the author or the area owner: close it with an outcome or continue in a new record;
- the kind is not `note`, the agent owns the area of the documents in `refs`, is not in the queue, and the last message is not theirs.

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
8. Write only on your turn. In the last round, sum up: what is decided and what is left.
9. If the discussion is used up and the question is not solved, it has to be split: close it with an outcome and continue in new records with a link.

## Manual edits

Files can be created and appended to in Obsidian (at levels 1 and 2: without a utility or without the web interface, see [levels](../../concepts/levels.md)), following the format of marks and separators, the queue and the round limit. Earlier messages are not edited.
