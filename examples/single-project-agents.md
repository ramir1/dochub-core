---
type: doc
lang: en
source: single-project-agents.ru.md
source_updated: 2026-11-06
status: draft
updated: 2026-11-07
---

# Example: several agents in one project

Case A from [use-cases](../concepts/use-cases.md#a-several-agents-in-one-project). The `portal` project: own documentation in the wiki, the repository's technical documentation connected as `docs/tech`.

## Members

| Agent | Tool | Machine | Area |
|---|---|---|---|
| `ramir-claude-backend` | Claude Code | laptop | `wiki/api` (owner) |
| `ramir-codex-frontend` | Codex | laptop, same repository | `wiki/ui` (owner) |
| `ramir-claude-tests` | Claude Code | development server | — (member) |
| `ramir` | person, Obsidian | laptop | default project owner |

The hub configuration is as in the [hub-config](../schemas/hub-config.md#example) example, plus the agent `ramir-claude-tests` (`role: participant`).

## Setup

**The portal repository**, `.doc-hub.yaml`:

```yaml
hubs:
  main:
    path: ~/projects/hub
    projects: [portal]
    agent: ramir-claude-backend
```

The default **agent name** comes from the manifest: `ramir-claude-backend` for the Claude Code session on the backend. Codex has different permissions, so its name is given in full: `DOC_HUB_AGENT=ramir-codex-frontend`. The manifest is local, and the development server has its own. Both work in one repository, but in the hub they are different agents.

**The hub** has clones on the laptop and the development server, with a shared origin. `config.local.yaml` on each machine holds its own path to the portal clone. Each machine updates the `docs/tech` export itself; `ramir` reads everything in Obsidian.

## Scenario

1. **The backend changes the error format.** `ramir-claude-backend` checks claims on `wiki/api/errors.md` (none), sets its own, changes the API in the repository and the document in the wiki.
2. **Notification.** It creates a `note` discussion "New API error format" with `to: [ramir-codex-frontend]` and `refs: [portal:wiki/api/errors.md]`. It releases the claim and pushes the hub. The files are as in the [schemas/discussion](../schemas/discussion.md#example) example.
3. **The frontend finds out.** A new Codex session starts by checking the inbox: it sees the `note`, reads `wiki/api/errors.md`, updates the client and closes the discussion with an outcome.
4. **Tests on another machine.** `ramir-claude-tests` starts a session on the server: `git pull` of the hub, a check of changes shows the change to `wiki/api/errors.md`; through `refs` it finds the discussion and outcome, understands why it changed, and updates the tests.
5. **A month later** an agent working with `wiki/api/errors.md` for the first time searches discussions by this document and finds the outcome, without asking a person.

## A question to another area

`ramir-claude-tests` believes that `wiki/ui/forms.md` describes the wrong behavior. This is the area of `ramir-codex-frontend`, so the tests agent does not edit the document but creates a `proposal` with `refs: [portal:wiki/ui/forms.md]`. The area owner accepts, edits the document in the wiki and marks it `applied` with the hub commit.
