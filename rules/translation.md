---
type: doc
lang: en
source: translation.ru.md
source_updated: 2026-11-07
status: draft
updated: 2026-11-07
---

# Translation

## Order

1. Core documents are written in Russian first: `<name>.ru.md`.
2. The English version is a separate file next to it, without a suffix: `<name>.md`.
3. Translation is done when the Russian version has reached `status: actual` (or by the author's decision).

## Linking versions

In the translation's frontmatter:

| Field | Value |
|---|---|
| `lang` | `en` |
| `source` | source file name: `overview.ru.md` |
| `source_updated` | the source's `updated` value at the time of translation |

If the source's `updated` is newer than the translation's `source_updated`, the translation is out of date and needs updating.

## Links

Links in a translation lead to files of the same language (`architecture.md`). If there is no translation yet, to the Russian version.

## Skills and templates

The file without a suffix is the working one: a tool reads it or an implementation copies it (`SKILL.md`, `templates/hub-AGENTS.md`). The Russian version next to it (`SKILL.ru.md`, `templates/hub-AGENTS.ru.md`) lets a person understand what is written there; tools do not use it.

These files do not get the `source` and `source_updated` fields: they are used as they are. On a change, both versions are edited.

## Terms

Term translations are recorded in the English column of the [glossary](../concepts/glossary.md).
