---
type: doc
lang: en
source: changes.ru.md
source_updated: 2026-11-06
status: draft
updated: 2026-11-07
---

# The "Changes" feature

**Key:** `changes` · **Status:** draft

Answers an agent's question: **what changed in the project documentation since my last view?** It is needed when an agent starts a new session, and also by the recipient of a cross-project contract, to learn that the contract has changed.

Wiki changes come from the **hub history**, docs changes from the **source repository history** (the local clone). The hub stores only each agent's **view mark**. A docs export contains no history.

## Model

A **view mark** holds the latest commits of the hub and of each source up to which the agent has seen changes. File `projects/<id>/changes/<agent>.yaml`, written only by the agent itself. Schema: [schemas/changes](../../schemas/changes.md).

**Changes** are commits after the mark that touch documents available to the agent (taking `members.<agent>.paths` into account).

## Operations

| Operation | What it does |
|---|---|
| show | A list of commits after the mark and the documents changed in them (on available paths) |
| show diff | The same with text changes |
| mark as viewed | Writes the branch's current latest commit to the mark |

Without a mark, the latest commits on available paths are shown with a note that there is no mark, or changes since a given commit. A window in days does not work: documentation may not change for weeks, and a new agent would see "no changes".

## Local folder

A local folder source has no history: changes to its files are not shown, and the view mark does not store them. The state is shown by comparing the folder with the export, see [docs-sources](../docs-sources.md#updating).

## Without a clone

If an agent has only a docs export, without a repository clone:

- the passport's `commit` differs from the mark, which means the documentation has changed;
- a list of changed files and a diff cannot be obtained: the export has no history. They require repository access.

## Link with discussions

- It is useful for the changes output to show discussions with status `applied` whose `commit` falls into the range: "why it changed".
- If a change affects other agents, the author of the edit creates a `note` (see [rules/agents](../../rules/agents.md#during-work)). `changes` does not replace the note; it is a safety net in case the note is forgotten.

## Limitations

- An agent without a clone of a source does not see that source's docs changes. Wiki changes are visible to everyone with a hub clone.
