---
type: doc
lang: en
source: default-rules.ru.md
source_updated: 2026-11-06
status: draft
updated: 2026-11-07
---

# Documentation rules

## Wiki requirements

Mandatory for the [wiki](../specifications/wiki.md): here the hub is the source of truth.

1. **Your own area.** Edit documents of your area (see [projects](../specifications/projects.md#areas)). For someone else's area, make a proposal to the owner through a discussion.
2. **Links.** Relative Markdown links leading to existing files. They are checked during data validation, see [protocol](../specifications/protocol.md#data-validation).
3. **Names** follow [naming](naming.md).
4. **One document, one topic.** A new topic goes in a new file with a link from the table of contents, if there is one.
5. **Keep the structure.** Do not rename or move files unnecessarily. If you moved one, update all links.
6. **Follow what the document has adopted:** style, language, section numbering, format of tables and examples.
7. **Reliability:** see [writing](writing.md).
8. **Relevance date.** If there is a version or date line ("Current as of..."), update it on a substantive edit.
9. **Notify.** If an edit affects other agents (contract, format, behavior), send a `note` to the addressees.

Frontmatter and `uid` are not required yet (*proposal*).
