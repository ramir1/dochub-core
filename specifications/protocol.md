---
type: doc
lang: en
source: protocol.ru.md
source_updated: 2026-11-06
status: draft
updated: 2026-11-07
---

# DocHub protocol

The protocol is what everyone who reads and writes hub data must follow: an agent at level 1, a utility, Web, a service. An implementation is compatible if any other implementation can read and change its data. Levels: [levels](../concepts/levels.md).

## What it includes

| Section | Where |
|---|---|
| Hub structure, synchronization, write collisions | [hub-storage](hub-storage.md) |
| Projects, areas | [projects](projects.md) |
| Agents, roles, permissions, visibility | [access](access.md) |
| Wiki | [wiki](wiki.md) |
| External documentation | [docs-sources](docs-sources.md) |
| Documents and links | [documents](documents.md) |
| Repository manifest | [project-manifest](project-manifest.md) |
| Features | [discussions](features/discussions.md), [changes](features/changes.md), [claims](features/claims.md) |
| Format version | [versioning](versioning.md) |
| File formats | [schemas/](../schemas/) |

## What it does not include

- The command interface: [cli-contract](cli-contract.md) describes the reference implementation `doc-hub`.
- Network API, authentication, MCP, deployment: these belong to a specific implementation.
- Default values of the reference implementation: claim duration, number of commits at the first view, number of write retries.

## Mandatory rules

An implementation must:

- write files strictly by the schemas and not lose unknown fields;
- not write to a hub with an unknown `format_version`;
- check roles, areas, path restriction and status transitions, taking the [agent suffix](access.md#suffix) into account;
- not change external documentation and not add it to the hub's git;
- not lose other people's data in a write collision, see [hub-storage](hub-storage.md#write-collisions).

## Context resolution

| What | Order |
|---|---|
| Hub | Explicit implementation parameter → `DOC_HUB_PATH` → `DOC_HUB_ID` → first section of `hubs` in the nearest `.doc-hub.yaml` going up from the current folder |
| Agent | Explicit parameter → `DOC_HUB_AGENT` → `agent` of the hub section with `DOC_HUB_AGENT_SUFFIX`; if the name is not defined, see [access](access.md#identity) |
| Project | Explicit parameter → first in `projects` of the hub section → first in `access` of the hub section → the only project where the agent is a member |

## Data validation

Data validation checks:

- **configuration:** `format_version`, whether agents from owners and members are described, whether roles are valid, whether owners are `participant` of the project;
- **discussions:** file name, required fields, `uid`, status for the kind, `outcome` when the status is not `open`, `changes` and `commit` when `applied`, an addressee for `note`, UTC time, message marks and separators, non-empty messages;
- **claims and view marks:** required fields, the link between file name and agent, UTC time;
- **wiki:** relative links lead to existing files.
