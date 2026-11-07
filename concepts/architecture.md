---
type: doc
lang: en
source: architecture.ru.md
source_updated: 2026-11-06
status: draft
updated: 2026-11-07
---

# Architecture

Layers and work levels: [levels](levels.md).

## Components

Terms: [glossary](glossary.md).

| Component | Where it is described |
|---|---|
| **Core** | [README](../README.md) |
| **Hub** | [hub-storage](../specifications/hub-storage.md) |
| **Wiki** | [wiki](../specifications/wiki.md) |
| **Docs** and project repositories | [docs-sources](../specifications/docs-sources.md) |
| **Features** | [features](features.md) |
| **Implementations** | [levels](levels.md), [protocol](../specifications/protocol.md) |
| **Agents** | [access](../specifications/access.md) |

## Diagram

```
  Machine 1                                 Machine 2
  ┌──────────────────────────────┐          ┌──────────────────────────────┐
  │ portal/ (repository)         │          │ mobile/ (repository)         │
  │   docs/                      │          │   .doc-hub.yaml              │
  │   .doc-hub.yaml              │          │                              │
  │ agents: backend (Claude),    │          │ agent: mobile-dev            │
  │         frontend (Codex)     │          │                              │
  │ hub/ (clone) ◄ agent/Obsidian│          │ hub/ (clone) ◄ agent         │
  │   projects/portal/docs/tech/ │          │                              │
  │     ◄─ export from portal/   │          │                              │
  └──────────────┬───────────────┘          └──────────────┬───────────────┘
                 │ git pull / push                         │ git pull / push
                 ▼                                         ▼
            ┌───────────────── hub origin ────────────────────┐
            │ .doc-hub/config.yaml  — projects, agents, access│
            │ projects/portal/wiki/ — own documentation       │
            │ projects/portal/discussions/ claims/ changes/   │
            └─────────────────────────────────────────────────┘
  projects/*/docs/ are separate on each machine and do not reach the hub origin.
```

## Flows

- **The wiki** is edited in the hub: by a person in Obsidian, by an agent through an implementation or by the skill.
- **Docs** are edited where they live; an explicit export copies them to the hub, and they do not reach the hub's git.
- **Features:** an agent works in its own repository, writes files to the hub clone, commits and pushes to origin.
- **Synchronization:** `git pull` before reading, `commit` and `push` after writing; collisions are covered in [hub-storage](../specifications/hub-storage.md#write-collisions).

## Outside Core

A service with an API, Web, an indexer and search, deployment (Docker and so on) and authentication belong to implementations, see [levels](levels.md).
