---
type: doc
lang: en
source: projects.ru.md
source_updated: 2026-11-08
status: draft
updated: 2026-11-08
---

# Projects

## What a project is

A **project** is a unit of documentation registered in the hub. The hub knows about a project:

| Parameter | Required | Description |
|---|:-:|---|
| identifier (`projects.<id>`) | yes | `^[a-z0-9][a-z0-9-]*$`, unique in the hub; matches the `projects/<id>/` folder |
| `name` | yes | Name |
| `docs.<folder>` | no | Connected external documentation, see [docs-sources](docs-sources.md) |
| `owners` | yes | Default project owners, for paths not covered by areas |
| `areas` | no | Areas and their owners |
| `members` | yes | Agents, their roles and path restrictions, see [access](access.md) |
| `features` | no | Enabled features, see [features](../concepts/features.md) |

Schema: [hub-config](../schemas/hub-config.md). Registration means adding the project to `.doc-hub/config.yaml`.

A project can have only a wiki, only docs, or both.

## Two kinds of documentation

| | [Wiki](wiki.md) | [Docs](docs-sources.md) |
|---|---|---|
| What it is | Own documentation | External documentation |
| Source of truth | Hub | Repository |
| Where it lives | `projects/<id>/wiki/` | `projects/<id>/docs/<folder>/` |
| In the hub's git | Yes | No |
| Editing | Obsidian (levels 1–2: without a utility or without the web interface), implementation, skill | Only in the repository |
| Hub requirements | Yes | No |

## Areas

An **area** is a path inside the documentation (a folder or file) with its own owners:

```yaml
owners: [ramir-claude-backend]
areas:
  wiki/api:        { owners: [ramir-claude-backend] }
  wiki/ui:         { owners: [ramir-codex-frontend] }
  wiki/mobile-app: { owners: [ramir-claude-backend] }
```

- A document belongs to the area with the **longest matching** path. If it matches none, it belongs to the project owners (`owners`).
- **Area owners** decide on proposals to change its documents and edit them in the wiki.
- Areas do not restrict reading or taking part in discussions: `members.<agent>.paths` exists for that.

## Cross-project contract

Documentation describing the interface between projects (for example, `mobile-app`) is stored **in the contract owner project's wiki** as an area. An agent of the recipient project is a member of that project with a `paths` restriction: it reads the contract and discusses it, and the area owner edits it. The contract arrives with the hub clone, and access to the owner project's code is not needed.

## Disconnection and removal

Not specified. Proposal: when a feature is disconnected, its data stay in the hub read-only; when a project is removed, the `projects/<id>/` folder is moved to an archive.
