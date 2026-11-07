---
type: doc
lang: en
source: levels.ru.md
source_updated: 2026-11-06
status: draft
updated: 2026-11-07
---

# Work levels

## Layers

| Layer | What it is | Where |
|---|---|---|
| Concept | Notions: wiki, docs, areas, agents, features | `concepts/` |
| Protocol | Mandatory compatibility rules | [protocol](../specifications/protocol.md), `specifications/`, `schemas/` |
| Implementations | CLI, Web, MCP, your own tools | Separate repositories |

DocHub is a concept and a protocol. CLI and Web are add-ons: they can be rewritten to the protocol, or you can write your own. `doc-hub` is the reference implementation, not a required part.

There is one protocol, the file protocol. The concept does not describe a network protocol: a service with an API is an implementation, and its API belongs to it.

## Levels

The level is defined by what stands between the agent and the hub files.

| Level | What exists | How the agent works | What is enforced |
|---|---|---|---|
| 1. Core | Core and hub files | Reads and writes files itself by the [skill](../skills/dochub/SKILL.md) | Instructions only |
| 2. Utility | Plus an implementation on the agent's machine (`doc-hub`) | Through commands | The utility checks permissions, formats, statuses |
| 3. Service | An implementation with its own API on top of the hub | Through that implementation's client, no files on the machine | The implementation |

At levels 1 and 2 a person works with the files directly in Obsidian.

### Level 1. Core

To start you need only Core and a hub folder. The agent gets direct access to the files and works by the skill.

The skill explains rather than forbids: permissions, areas and formats rest on the agent's discipline. Errors in fields, times and `uid`, as well as skipped steps, are possible. The result can be checked by any implementation of data validation, see [protocol](../specifications/protocol.md#data-validation).

### Level 2. Utility

The implementation removes manual steps: it sets `uid` and times itself, checks permissions and status transitions, and synchronizes the hub. The agent works only through it.

This protects against mistakes, not against deliberate bypass: the hub files are on the agent's machine, and technically access to them remains.

### Level 3. Service

The hub files are only on the server, and the agent gets what the service returns. Only here are real verification of the agent and its permissions, a private part of the wiki and delivery of docs to agents without repository access possible.

Core does not describe how this works. The service stores data by the file protocol, so it is compatible with other implementations at the hub level.

## Hub storage

This is not a level but a method shared by levels 1 and 2:

| Method | What it is | Limits |
|---|---|---|
| Folder | The hub on one machine, without origin | One machine |
| Git server | The hub repository on a server, clones on machines | Several machines; a clone exposes the whole wiki |

Docker is not a separate level: a mounted folder or git in a container is storage at levels 1 and 2, and an API in a container is level 3.

## Moving between levels

The file format is the same at all levels, so moving requires no data transfer. To go from level 1 to level 2, it is enough to install an implementation. For level 3, deploy a service on top of the same hub.
