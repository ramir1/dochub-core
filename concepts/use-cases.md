---
type: doc
lang: en
source: use-cases.ru.md
source_updated: 2026-11-07
status: draft
updated: 2026-11-07
---

# Use cases

## A. Several agents in one project

The main case. Several agents work on one repository:

- **different tools**: Claude Code and Codex in one repository;
- **parallel sessions by area**: separate agents for backend, frontend, tests;
- **different machines and people**: agents of different developers;
- **executor and reviewer**: usually this is one session of one agent, and no hub is needed; if they are separated, the reviewer writes questions and proposals through discussions.

The documentation is available to them: their own in the wiki, external in docs. They need coordination:

| Need | How the hub solves it |
|---|---|
| "I changed the API response format, check the client" | A discussion of kind `note` addressed to the frontend agent |
| "Why was it done this way?", a week later, in another tool | Discussion outcomes by document (`refs`) |
| "What changed in `wiki/api/` since my last session?" | The `changes` feature |
| Two agents take on the same section | The `claims` feature: a "working on `wiki/api/auth.md`" mark |
| A proposal to change someone else's area | A `proposal` discussion, decided by the area owner |
| Sharing work among agents and seeing what is done | The `board` feature: a card with an assignee, a status and an outcome |

Examples: [examples/single-project-agents](../examples/single-project-agents.md), [examples/board-agents](../examples/board-agents.md).

## B. Cross-project contract

**Example:** the documentation "Portal external perimeter for the mobile app" (`mobile-app`). It describes the portal API for the mobile team and is stored **in the portal project's wiki** (`wiki/mobile-app/`): it is edited from one place and arrives with the hub clone.

| Agent | Where it works | Permissions in the `portal` project |
|---|---|---|
| backend agent | portal repository | participant; **owner of the area** `wiki/mobile-app/`; edits the documentation in the wiki |
| mobile app agent | its own repository, without access to the portal code | participant with access **only to the path** `wiki/mobile-app/` |

The mobile agent reads the contract and suggests changes through a `proposal`, the area owner decides and edits the wiki, and the recipient sees the edit through `changes`.

The recipient does not edit the documentation: a contract change goes through agreement. There is no separate "contract project": it is an area in the contract owner project.

Example: [examples/contract-zone](../examples/contract-zone.md).

## C. Reading another project's documentation

A project is registered in the hub so that agents of other projects and people can read its documentation together with the documentation of other projects (in Obsidian or through an implementation) as docs, see [docs-sources](../specifications/docs-sources.md). No features are needed. The agent is given the `reader` role, with a path restriction if necessary.

This is a special case: by itself it gives little (reading can also be arranged with a repository clone), but it requires nothing from the project except registration.
