---
type: doc
lang: en
source: AGENTS.ru.md
source_updated: 2026-11-06
status: draft
updated: 2026-11-07
---

# Instructions for agents working with Core

This file is for agents that **edit Core itself** (the DocHub specifications). Instructions for agents working with a **hub** are in [rules/agents](rules/agents.md) and the [hub-AGENTS](templates/hub-AGENTS.md) template.

## Rules

1. **Do not invent.** Describe only what is agreed or follows from the sources. If there is no information, say so; mark what is not agreed as **"proposal"**, what is not decided as **"open question"**, and add it to the [list](concepts/overview.md#open-questions).
2. **Consistency.** One field or concept has the same name everywhere. When changing a field, update together: the specification (`specifications/`), schema (`schemas/`), examples (`examples/`), templates (`templates/`).
3. **Terms** come from the [glossary](concepts/glossary.md). Add a new term there first.
4. **Language.** New documents are written in Russian, with the `.ru.md` suffix. For English versions, see [translation](rules/translation.md).
5. **Names** follow [naming](rules/naming.md).
6. **Links** are relative Markdown links, so that they work in Obsidian, on GitHub and in Web.
7. **Frontmatter** of a Core document: `type: doc`, `lang`, `status` (`draft` | `actual` | `outdated`), `updated` (`YYYY-MM-DD`).
8. **Core boundaries.** Core describes the concept and the file protocol, not implementations: a network API and how applications work do not belong here. Implementation commands are described only in [cli-contract](specifications/cli-contract.md): it is an example of an implementation, not part of the protocol. Other documents name operations ("writing to the wiki", "exporting docs"), not commands. The hub is the source of truth only for the wiki; it does not edit external documentation (docs) or store it in git. Core does not dictate how a project manages tasks.
