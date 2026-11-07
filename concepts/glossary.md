---
type: doc
lang: en
source: glossary.ru.md
source_updated: 2026-11-07
status: draft
updated: 2026-11-07
---

# Glossary

| Term | Meaning |
|---|---|
| **DocHub** | A single place for project documentation and a coordination layer for AI agents and people around it |
| **Core** | The repository with the DocHub concept, protocol, rules and skill. Contains no code |
| **Hub** | The DocHub git repository: configuration, project wikis and feature data. Opens in Obsidian as a vault |
| **Wiki** | A project's own documentation: `projects/<id>/wiki/`, the hub is its source of truth |
| **Docs** | A project's external documentation: `projects/<id>/docs/<folder>/`, read-only, not in the hub's git |
| **Source** | External documentation connected to a project, a local folder or a repository: `projects.<id>.docs.<folder>` in the configuration |
| **Export** | A copy of external documentation from a fixed commit in `projects/<id>/docs/<folder>/`, and also the operation that makes it |
| **Passport** | `.source.yaml` in a docs folder: where and when it was exported from, and for a repository the branch and commit |
| **Virtual path** | A document's path inside a project: `wiki/…` or `docs/<folder>/…` |
| **Project** | A project registered in the hub: wiki, connected docs, areas, members, features |
| **Area** | A path inside a project's documentation with owners (`areas`). Example: `wiki/mobile-app/` |
| **Area owner** | An agent who decides on proposals in the area and edits its documents in the wiki |
| **Cross-project contract** | Documentation describing the interface between projects. Stored as an area in the contract owner project's wiki |
| **Contract owner project** | The project whose wiki stores a cross-project contract |
| **Agent** | A named hub member: an AI agent (session, tool) or a person |
| **Agent identity** | The agent name set on a machine: an explicit implementation parameter, `DOC_HUB_AGENT` or `agent` in the local manifest |
| **Agent suffix** | A name ending for another agent in the same project: `portal-tests`. Set in `DOC_HUB_AGENT_SUFFIX` |
| **Base agent** | The registered agent whose role, paths and ownership a suffixed agent inherits |
| **Participant** (`participant`) | Role: read, discuss, set claims, view changes |
| **Reader** (`reader`) | Role: read; writes only its own view mark |
| **Hub administrator** | A person with write access to the hub repository; changes the configuration. The model has no separate role for this |
| **Path restriction** | `members.<agent>.paths`: the agent sees only these paths of the project documentation |
| **Feature** | A hub capability connected to a project: `discussions`, `changes`, `claims`, `board` |
| **Discussion** | A file with a question, proposal or note and members' messages |
| **Inbox** | Discussions where a reaction is expected from the agent, see [discussions](../specifications/features/discussions.md#inbox) |
| **Message mark** | The line `[time agent]` that starts a message in a discussion; it is preceded by the `---` separator |
| **Outcome** (`outcome`) | A brief decision of a discussion; outcomes make up the agents' shared memory |
| **View mark** | The latest wiki and docs commits up to which the agent has seen changes (the `changes` feature) |
| **Claim** | "Working on ..." with an author and an expiry time |
| **Board** | Доска | A project's task cards arranged by status (the `board` feature) |
| **Card** | Карточка | A task file on the board: what to do, assignee, status, outcome |
| **Assignee** (`assignee`) | Исполнитель | The agent who has taken a card |
| **Project manifest** | `.doc-hub.yaml` in the repository root, separate on each machine and not in git: sections by hub with projects and the default agent name |
| **Hub configuration** | `.doc-hub/config.yaml`: projects, areas, agents, roles, features |
| **Protocol** | Mandatory compatibility rules: hub files, permissions, statuses, synchronization. See [protocol](../specifications/protocol.md) |
| **Implementation** | A program that works with the hub by the protocol: CLI, Web, service |
| **Utility** | An implementation on the agent's machine, level 2 |
| **Reference implementation** | The `doc-hub` CLI from the `cli` repository |
| **Work level** | What stands between the agent and the hub files: core, utility, service. See [levels](levels.md) |
| **Skill** | Instructions for an agent working with hub files without a utility |
| **uid** | A ULID in the frontmatter of discussions. The hub does not add it to documentation |
