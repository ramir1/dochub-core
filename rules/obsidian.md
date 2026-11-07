---
type: doc
lang: en
source: obsidian.ru.md
source_updated: 2026-11-07
status: draft
updated: 2026-11-07
---

# Working through Obsidian

Obsidian is the hub's interface for people: the hub opens as a vault, one for all projects. It shows and lets you edit the wiki, and shows discussions, claims and external documentation.

## What you can do in Obsidian

Obsidian does not know the hub's rules: nothing checks permissions, areas or links, and `.doc-hub/`, `.source.yaml` and `changes/` are not visible in it. The same guide lies in the hub root in two languages: `OBSIDIAN.md` and `OBSIDIAN.ru.md`.

| Action | Allowed in Obsidian | What happens | The right way |
|---|---|---|---|
| Read wiki, docs, discussions | Yes | — | — |
| Edit text in `wiki/` | Yes, in your area | The Git plugin pushes the edit | — |
| Rename a file or folder in `wiki/` | With care | Links inside the wiki update, but areas and agent paths in the configuration stop matching, and links from discussions go stale | Fix `.doc-hub/config.yaml` afterwards |
| Edit a file in `docs/` | No | The edit is lost on the next export | Edit it in the source |
| Rename a folder in `docs/` | No | The folder becomes an orphan, and the export creates the old one again | Disconnect the source and connect it under the new name |
| Append a message to a discussion | Yes, following the format | — | See [Discussions](#discussions) |
| Change a discussion status | By hand | Edit the frontmatter and add a mark; nothing checks the transition | Better through an implementation |
| Rename a discussion file | No | The old name stops working | Do not rename |
| Change `.doc-hub/`, `.source.yaml`, `changes/` | Not visible | — | The hub administrator, in a regular editor |

After manual edits, run data validation, see [protocol](../specifications/protocol.md#data-validation). It finds format errors and broken wiki links, but not orphan folders in `docs/` or areas without files.

## Vault setup

1. Clone the hub: `git clone <hub origin> ~/projects/hub`. Open the folder in Obsidian: "Open folder as vault".
2. **Files and links:**
   - "Automatically update internal links": on;
   - "Use [[Wikilinks]]": **off**;
   - "New link format": relative path.
3. **Core plugins:** "Templates" (folder: `templates`), "Properties", "Search", "Backlinks".
4. **Community plugins:**
   - **Git** (obsidian-git): automatic pull on open and push after changes; the commit message carries the human agent's name (`ramir: …`);
   - **Note UID Generator**: for discussion `uid`s, see below.

## Note UID Generator

Hub files are written by different authors on different machines. Automatic `uid` generation would change someone else's file on opening, so it is **off**; the `uid` is set by hand with the plugin command in your own new discussion file.

| Key (`.obsidian/plugins/note_uid_generator/data.json`) | Value |
|---|---|
| `uidKey` | `uid` |
| `uidGenerator` | `ulid` |
| `autoGenerateUid` | `false` |
| `duplicateUidCheck` | `notify` |

## Wiki

Own documentation is in `projects/<id>/wiki/`. Edit right in Obsidian, and the Git plugin pushes the changes. Edit documents of your area, requirements are in [default-rules](default-rules.md#wiki-requirements). Another machine may have changed the same file: on a conflict the Git plugin shows it, and it is resolved like an ordinary git conflict.

## External documentation

`projects/<id>/docs/<folder>/` is read-only, see [docs-sources](../specifications/docs-sources.md).

- **Update** with the implementation's export operation, see [docs-sources](../specifications/docs-sources.md#updating).
- **Do not edit.** An edit is lost on the next update and does not reach the repository. The folder is in the hub's `.gitignore`, and the Git plugin does not push it.
- **Freshness.** The commit is in `.source.yaml`. Obsidian hides this file (the name starts with a dot): look at it outside Obsidian or through an implementation.
- **Search:** `path:"projects/portal/wiki"` or `path:"projects/portal/docs"`.

## Discussions

- **Read:** `projects/<id>/discussions/<file>.md`, the whole conversation in one document.
- **Open discussions:** search `path:"projects/portal/discussions" [status:open]`.
- **My inbox:** search `[to:ramir]`.
- **Create:** a new file `YYYY-MM-DD-slug.md` from the `templates/discussion.md` template, then the plugin's "generate UID" command.
- **Reply:** append to the end of the file a blank line, the `---` separator, the mark `[YYYY-MM-DDTHH:MM:SSZ <agent>]` and the text. Do not change other people's messages.
- Time in templates is local, while the schema requires UTC: replace it with UTC time on creation. This is an inconvenience of stage 1.

## Claims

`projects/<id>/claims/` shows who is working on what. A person can also set a claim from the `templates/claim.md` template. In the template `created` and `expires` are the same: convert the time to UTC and set the duration in `expires`, otherwise the claim expires immediately.

## Templates

Copy from Core to the hub's `templates/`: [discussion](../templates/discussion.md), [claim](../templates/claim.md). The Russian versions lie next to them: [discussion](../templates/discussion.ru.md), [claim](../templates/claim.ru.md).
