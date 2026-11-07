---
type: doc
lang: en
source: features.ru.md
source_updated: 2026-11-06
status: draft
updated: 2026-11-07
---

# Hub features

A **feature** is a hub capability that can be connected to a project. Without features a project has only documentation: wiki and docs (case C, see [use-cases](use-cases.md)).

## Common properties

- A feature has a specification in [`specifications/features/`](../specifications/features/) and a file schema in [`schemas/`](../schemas/).
- It is enabled for a project in the hub configuration: `projects.<id>.features.<key>`, see [hub-config](../schemas/hub-config.md).
- Data live in the hub: `projects/<id>/<key>/`. Features write nothing to the project repository.
- Work depends on the level: through an implementation or by the skill; people work in Obsidian.
- Permissions follow roles and area owners, see [access](../specifications/access.md).

## List

| Feature | Key | What it gives | Specification |
|---|---|---|---|
| Discussions | `discussions` | Channel and shared memory: questions, proposals, notes, outcomes | [discussions](../specifications/features/discussions.md) |
| Changes | `changes` | What changed in the documentation since the agent's last view | [changes](../specifications/features/changes.md) |
| Claims | `claims` | Who is working on what right now | [claims](../specifications/features/claims.md) |

New features are added when a real use case appears.
