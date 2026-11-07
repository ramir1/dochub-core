---
type: doc
lang: en
source: naming.ru.md
source_updated: 2026-11-07
status: draft
updated: 2026-11-07
---

# Naming

## Files and folders of Core, the hub and implementations

- Lowercase Latin letters, digits, hyphen: **kebab-case** (`project-manifest.md`, `use-cases.md`).
- No spaces, Cyrillic, underscores or double extensions (`README.md.md` is an error).
- Sequence numbers go at the start: `01-quick-start.md`, `02-domains.md`.
- Service files are uppercase: `README`, `AGENTS`.

## Language suffix

Documentation is written in Russian. The Russian version of a Core, hub or implementation document has the `.ru` suffix before the extension, and the English version has no suffix:

```
overview.ru.md    # Russian version
overview.md       # English version
```

The language code is ISO 639-1. More: [translation](translation.md).

Exceptions:

- Tools look for or copy `README.md`, `AGENTS.md`, `SKILL.md` and templates by exact name, so the working version of these files is English. The Russian one lies next to it for people: `README.ru.md`, `AGENTS.ru.md`, `SKILL.ru.md`, `templates/hub-AGENTS.ru.md`.
- The guide in the hub root is uppercase: `OBSIDIAN.md` and `OBSIDIAN.ru.md`.
- Skills are `skills/<name>/SKILL.md`: the file name is set by the skill format.
- Project documentation follows the project's own rules; existing files are not renamed for the sake of a suffix.

## Identifiers

Identifiers of the hub, projects, agents, feature keys: `^[a-z0-9][a-z0-9-]*$` (`portal`, `ramir-claude-backend`, `discussions`).

## Agents

Recommended name: `<person>-<tool>-<area>`:

- `ramir-claude-backend`, `ramir-codex-frontend`, `ramir-claude-tests`;
- a person is just a name: `ramir`;
- an agent of another team goes by project: `mobile-dev`;
- another agent in the same project gets a suffix: `portal-tests`, see [access](../specifications/access.md#suffix).

## Hub data

Location and file names: [hub-storage](../specifications/hub-storage.md#structure). Wiki documents are named as is customary in the project, in Latin letters in kebab-case. `slug` is the topic in Latin letters in kebab-case, 2–5 words.
