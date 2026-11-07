---
type: doc
lang: en
source: docs-sources.ru.md
source_updated: 2026-11-06
status: draft
updated: 2026-11-07
---

# External documentation (docs)

**Docs** are external documentation connected to a project, **read-only**. The hub does not edit them and only exports them for reading. Own documentation: [wiki](wiki.md).

## Source kinds

| | Local folder | Repository |
|---|---|---|
| When | By default: the documentation is on this machine | Explicitly: a fixed commit or another repository is needed |
| What is taken | Files as they are, including uncommitted ones | A fixed commit of the clone's local branch |
| Git needed | No | Yes |
| Commit in the passport | No | Yes |
| Change history | No | Yes |

## Connecting

A source is connected in the project configuration: the key is the folder name, the value is where to take it from. With `repository` it is a repository source, without it a local folder.

```yaml
projects:
  portal:
    docs:
      tech:                                  # local folder
        path: packages/ramirko/postcode/docs
      api:                                   # repository
        repository: git@github.com:example/portal.git
        branch: main
        path: docs
```

The contents of `path` go straight into the folder: `docs/tech/` gets the files from `path`. There can be several sources. The root on this machine is `projects.<id>.docs.<folder>.local_path` in `config.local.yaml`: the project root for a local folder, the clone for a repository. `path` is given from this root, so it is the same on all machines.

**Archive.** A level 3 implementation may accept documentation as an uploaded archive: `docs.<folder>: {upload: true}`. There is no source outside the hub, and the files exist only in the export. It is updated by a new upload, and the export command skips such a source. The hub is not responsible for storing these files: they do not get into git or the backup.

## Properties

- **Read-only.** An export is not edited: an edit is lost on the next export. Edit in the source.
- **Not in the hub's git.** These are sources for reading. `projects/*/docs/` is in the hub's `.gitignore`, and each machine has its own export.
- **With a passport.** The folder holds a `.source.yaml` passport, see [schemas/docs-source](../schemas/docs-source.md).
- **No requirements.** The hub requires nothing of external documentation and does not change its files.

An agent without access to the source does not get the docs. Everything it needs to read goes into the wiki.

## Updating

**Local folder.** An export copies the folder's files as they are to `projects/<id>/docs/<folder>/` and writes the passport. It does not copy the `.git` folder. If the folder is not on this machine, the old export remains.

**Repository.** An export is done for all sources of the project or for selected folders:

1. Runs `git fetch` in the clone if it has an origin. An unavailable origin does not block the export.
2. Takes a **fixed commit** of the local branch `branch` or an explicitly given ref. The working copy is not used. The local branch is chosen because documentation may be committed but not yet pushed.
3. Exports `path` to a temporary folder and replaces `projects/<id>/docs/<folder>/` with it. If the commit has no such path, the old export remains.
4. Writes the passport. Commits nothing to the hub's git.

An agent with a path restriction gets only the allowed paths exported. On a shared machine such an export narrows for everyone, so it is better updated by an agent without restrictions.

Updating is explicit only: a person sets up running it at session start or from a git hook.

**Warnings** during an export and when showing its state:

- local folder: files differ from the export;
- repository: the export is behind the branch; the commit is not pushed to origin; the repository has uncommitted source changes;
- any source: the folder is not in the hub's `.gitignore`.

## Reading

A **local folder** is read directly, so the data are always fresh. If the folder is not on this machine, it is read from the export.

A **repository** is read from the export, and if there is none, from the commit in the clone. The implementation shows the source and commit. A link `project:path@commit` is read from the repository's git; a local folder has no commits.
