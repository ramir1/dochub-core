---
type: doc
lang: en
source: README.ru.md
source_updated: 2026-11-07
status: draft
updated: 2026-11-07
---

# DocHub Core

**DocHub** is a single place for project documentation and a coordination layer for AI agents (and people) around it. A project has its own documentation, the **wiki**, for which the hub is the source of truth, and connected external documentation, **docs**, which is read-only. Around them the hub gives agents a shared channel (discussions), shared memory (outcomes tied to documents), change notifications and marks showing who is working on what.

**Core** is this repository: the concept, protocol, rules and skill. There is no code here: implementations (CLI, Web, your own tools) work by the protocol and can be rewritten.

> **Status:** draft; the first version of the CLI is in the `cli` repository. **"Proposal"** means not finally agreed; **"open question"** means not decided.

## Where to start

1. [Overview](concepts/overview.md): the idea, principles, stages.
2. [Use cases](concepts/use-cases.md): what it is for in practice.
3. [Work levels](concepts/levels.md): concept, protocol, implementations.
4. [Architecture](concepts/architecture.md): what DocHub consists of.
5. [Integration](rules/integration.md): how to create a hub and connect a project.
6. [Rules for agents](rules/agents.md) and [working through Obsidian](rules/obsidian.md): how to use the hub today.

## File map

[AGENTS](AGENTS.md) holds instructions for agents that edit Core itself.

### concepts/: concepts

| File | About |
|---|---|
| [overview](concepts/overview.md) | The idea, principles, stages, open questions |
| [use-cases](concepts/use-cases.md) | Several agents in one project; a cross-project contract; reading another project's documentation |
| [levels](concepts/levels.md) | Layers, work levels, hub storage |
| [architecture](concepts/architecture.md) | Repositories, hub, implementations, agents, synchronization |
| [features](concepts/features.md) | Hub features: discussions, changes, claims, board |
| [glossary](concepts/glossary.md) | Terms |

### specifications/: specifications

| File | About |
|---|---|
| [protocol](specifications/protocol.md) | What the protocol includes and what an implementation must do |
| [projects](specifications/projects.md) | Project, wiki and docs, areas and owners |
| [access](specifications/access.md) | Agents, identity, roles, path restriction |
| [project-manifest](specifications/project-manifest.md) | The `.doc-hub.yaml` file in a repository |
| [hub-storage](specifications/hub-storage.md) | Hub storage and synchronization between machines |
| [wiki](specifications/wiki.md) | A project's own documentation |
| [docs-sources](specifications/docs-sources.md) | External documentation, read-only |
| [documents](specifications/documents.md) | Document identification and links |
| [versioning](specifications/versioning.md) | Format versions |
| [cli-contract](specifications/cli-contract.md) | Commands of the reference implementation `doc-hub`, not part of the protocol |
| [features/discussions](specifications/features/discussions.md) | Discussions: questions, proposals, notes |
| [features/changes](specifications/features/changes.md) | Documentation changes since the last view |
| [features/claims](specifications/features/claims.md) | "Working on ..." marks |
| [features/board](specifications/features/board.md) | Task board: cards, statuses, assignees |

### schemas/: formats

| File | About |
|---|---|
| [hub-config](schemas/hub-config.md) | `.doc-hub/config.yaml` and `config.local.yaml` |
| [project-manifest](schemas/project-manifest.md) | `.doc-hub.yaml` |
| [discussion](schemas/discussion.md) | Discussion file and message marks |
| [changes](schemas/changes.md) | View mark for changes |
| [claim](schemas/claim.md) | Claim |
| [card](schemas/card.md) | Board card |
| [docs-source](schemas/docs-source.md) | External documentation passport |

### rules/: rules

| File | About |
|---|---|
| [integration](rules/integration.md) | Creating a hub and connecting a project |
| [agents](rules/agents.md) | How an AI agent works with the hub |
| [obsidian](rules/obsidian.md) | Setting up and working through Obsidian |
| [default-rules](rules/default-rules.md) | Wiki requirements |
| [naming](rules/naming.md) | Names of files, folders, agents |
| [writing](rules/writing.md) | Style and reliability |
| [translation](rules/translation.md) | Translation ru → en |

### skills/: skills

| File | About |
|---|---|
| [dochub](skills/dochub/SKILL.md) | How an agent works with hub files without a utility (level 1) |

### examples/ and templates/

- Examples: [several agents in one project](examples/single-project-agents.md), [cross-project contract](examples/contract-zone.md), [a task board for agents](examples/board-agents.md).
- Templates: [`.doc-hub.yaml`](templates/doc-hub.yaml), [hub AGENTS.md](templates/hub-AGENTS.md), Obsidian guide ([en](templates/hub-obsidian.md), [ru](templates/hub-obsidian.ru.md)), [hub .gitignore](templates/hub-gitignore), [project instructions](templates/project-agents-snippet.md), [discussion](templates/discussion.md), [claim](templates/claim.md), [card](templates/card.md).

## Languages

Documents are written in Russian first (`*.ru.md`) and then translated into English (a file next to it, without a suffix: `*.md`). See [translation](rules/translation.md).
