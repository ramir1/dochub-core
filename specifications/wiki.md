---
type: doc
lang: en
source: wiki.ru.md
source_updated: 2026-11-07
status: draft
updated: 2026-11-07
---

# Project wiki

The **wiki** is a project's own documentation. The hub is its **source of truth**: the files live in `projects/<id>/wiki/` and are stored in the hub's git. The counterpart is external documentation, [docs](docs-sources.md), which is only read.

## What goes in the wiki

Contracts between projects and general descriptions: everything that needs to be edited from one place and read by agents without access to the code. What changes together with the code is better left in the repository and connected as docs.

## Paths

The virtual path matches the project folder: `wiki/mobile-app/09-comments.md` lives in `projects/portal/wiki/mobile-app/09-comments.md`. A link to a document is `portal:wiki/mobile-app/09-comments.md`, see [documents](documents.md).

## Who edits

- **A person**: directly in Obsidian, since the hub is the storage. Synchronization is done by the Git plugin.
- **An AI agent**: through an implementation, and at level 1 directly by the skill.
- Only the document's **area owner** can write, and outside areas the project owner. Others suggest an edit through a `proposal`.

## Requirements

The hub sets requirements for the wiki, see [default-rules](../rules/default-rules.md#wiki-requirements). Data validation checks links, see [protocol](protocol.md#data-validation). Frontmatter and `uid` are not required yet (*proposal*).

## Git service files

The files and folders `.git`, `.gitignore`, `.gitattributes`, `.gitmodules` are not written to the wiki: they would change how the hub git works, not the documentation. An implementation refuses such a path at any depth.

## Concurrent edits

Different agents and people can edit the same wiki document.

- An edit is written on top of the version that was read. An implementation may accept the commit the document was read from and reject the write if the file has changed in the meantime.
- If two machines write one file at the same time, the second gets a conflict error: its write is cancelled and the hub clone stays clean. The document must be read again.

## Visibility

The wiki is stored in the hub's git and is visible to everyone who has its clone: only an implementation checks `members.<agent>.paths`. A part of the wiki can be closed off with a separate hub or a level 3 service, see [levels](../concepts/levels.md).
