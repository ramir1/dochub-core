---
type: doc
lang: en
source: documents.ru.md
source_updated: 2026-11-06
status: draft
updated: 2026-11-07
---

# Documents

## Where documents are

A document is a Markdown file in the project's [wiki](wiki.md) or connected [docs](docs-sources.md).

## Identification

A document is given as **project + path in the project**: `wiki/…` or `docs/<folder>/…`:

```
portal:wiki/mobile-app/09-comments.md
```

If needed, with the commit the author had in mind:

```
portal:wiki/mobile-app/09-comments.md@4f2a9c1
```

In message text within one project, the project may be omitted.

The commit comes from the hub for the wiki and from the source repository for docs. The commit must be pushed to origin, otherwise the link will not open on other machines. A document may have changed after the given commit: read the current version in the documentation.

## uid

- The hub does **not add** `uid` or other fields to documentation.
- In hub data, discussions have a `uid` (ULID). It is set by the creator: an implementation, an agent or a person with an Obsidian plugin command. More: [discussion](../schemas/discussion.md).
- A `uid` is not changed or copied.

## Links

- In the wiki: relative Markdown links, see [default-rules](../rules/default-rules.md#wiki-requirements). In docs: by the rules of their repository.
- From hub data to documents: in the `refs` and `changes` fields in the `project:path` format; in message text the same way, in `monospace`. The `paths` field of a claim stores virtual paths without a project: the claim file already lies in the project.
- Obsidian wiki links (`[[...]]`) are not used in hub data: they do not work outside Obsidian.
- File paths on disk are not used in hub data.

## Moving documents

A wiki document is moved by the area owner, and data validation checks links to it inside the wiki. Links from hub data (`refs` of old discussions) do not update themselves: a discussion reflects the state at the time it was created. A docs document is moved in its repository.
