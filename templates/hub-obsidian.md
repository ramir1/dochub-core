# Working with the hub in Obsidian

Obsidian shows the hub files but does not know its rules: nothing checks permissions, areas or links, and some files are hidden. Copied to the hub root as `OBSIDIAN.md`.

| Action | Allowed in Obsidian | What happens | The right way |
|---|---|---|---|
| Read wiki, docs, discussions | Yes | — | — |
| Edit text in `wiki/` | Yes, in your area | The Git plugin pushes the edit | — |
| Rename a file or folder in `wiki/` | With care | Links inside the wiki update, but areas and agent paths in the configuration stop matching, and links from discussions go stale | Fix `.doc-hub/config.yaml` afterwards |
| Edit a file in `docs/` | No | The edit is lost on the next export | Edit it in the source |
| Rename a folder in `docs/` | No | The folder becomes an orphan, and the export creates the old one again | Disconnect the source and connect it under the new name |
| Append a message to a discussion | Yes, following the format | — | Separator `---`, mark, text; do not change earlier messages |
| Change a discussion status | By hand | Edit the frontmatter and add a mark; nothing checks the transition | Better use the utility |
| Change a card status | By hand | The Kanban view in Bases changes only `status`; nobody adds the history mark or the outcome | Better through the utility; after the edit add the mark and `outcome` |
| Rename a discussion file | No | The old name stops working | Do not rename |
| Change `.doc-hub/`, `.source.yaml`, `changes/` | Not visible | — | The hub administrator, in a regular editor |

After manual edits, run the hub data check. It finds format errors and broken wiki links, but not orphan folders in `docs/` or areas without files.

For commands, see the utility reference.
