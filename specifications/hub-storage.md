---
type: doc
lang: en
source: hub-storage.ru.md
source_updated: 2026-11-07
status: draft
updated: 2026-11-07
---

# Hub storage

The hub is a git repository with configuration, project wikis and feature data. Connected external documentation lies next to them but does not reach git. It is also an Obsidian vault, see [obsidian](../rules/obsidian.md).

## Structure

```
<hub>/
├── README.md                    # hub description for people
├── AGENTS.md, AGENTS.ru.md      # entry point for AI agents and its Russian version (templates: templates/hub-AGENTS*.md)
├── OBSIDIAN.md, OBSIDIAN.ru.md  # guide to working in Obsidian (templates: templates/hub-obsidian*.md)
├── .gitignore                   # .doc-hub/config.local.yaml, .obsidian/workspace.json, projects/*/docs/ (template: templates/hub-gitignore)
├── .doc-hub/
│   ├── config.yaml              # projects, areas, agents, roles, features (in git)
│   └── config.local.yaml        # paths to repository clones on this machine (not in git)
├── .obsidian/                   # Obsidian settings
├── templates/                   # templates for the Templates plugin
└── projects/
    └── <id>/
        ├── wiki/                     # own documentation, in git
        ├── docs/                     # external, not in git
        │   └── <folder>/
        │       └── .source.yaml      # passport
        ├── discussions/
        │   └── <YYYY-MM-DD-slug>.md
        ├── changes/
        │   └── <agent>.yaml          # view mark
        ├── claims/
        │   └── <agent>-<slug>.md     # claim
        └── board/
            └── <slug>.md             # card
```

File schemas: [discussion](../schemas/discussion.md), [changes](../schemas/changes.md), [claim](../schemas/claim.md), [card](../schemas/card.md), [docs-source](../schemas/docs-source.md).

## Who changes which file

Write permissions: [access](access.md#write-policy). By file:

| File | Who changes it |
|---|---|
| `discussions/<slug>.md` | members append messages; the status changes by the feature's rules |
| `changes/<agent>.yaml` | the agent itself |
| `claims/<agent>-<slug>.md` | the agent itself; any member may delete an expired claim |
| `board/<slug>.md` | participants, by the feature rules |
| `.doc-hub/config.yaml` | the hub administrator |
| `wiki/…` | the area owner |
| `docs/<folder>/…` | only the export operation; not in git |

Marks are laid out in their authors' files and do not conflict on merge. Discussions, cards and the wiki can collide, see [write collisions](#write-collisions).

## Synchronization between machines

The hub is an ordinary git repository with a shared remote (origin). Each machine has its own clone.

Write order for any implementation:

1. Before reading and writing: `git pull --rebase`.
2. After writing: `git add` and `commit` of your own files only. The message is `<agent>: <action> <object>`, and the commit author is the agent name.
3. `git push`. On rejection: `pull --rebase` and retry. If origin is unavailable, the local commit is kept and pushed later; the write is not cancelled.

People in Obsidian use the Git plugin (automatic pull/push).

**A hub without origin.** A valid mode for one machine: `pull` and `push` are skipped.

### Write collisions

If `rebase` ends in a conflict: abort it, undo your commit, update the hub.

- **Discussion:** append the message again on top of the fresh version. Before changing a status, check again that the transition is valid.
- **Card:** re-read it, check again that the transition is valid, and repeat the write.
- **Wiki:** refuse the write and report the conflict so that the author rereads the document.

Other people's changes are not overwritten. A person in Obsidian resolves a conflict manually.

Feature folders are created on the first write.

A service instead of git is a level 3 implementation, outside Core, see [levels](../concepts/levels.md).
