---
type: doc
lang: en
source: cli-contract.ru.md
source_updated: 2026-11-08
status: draft
updated: 2026-11-08
---

# Interface of the reference implementation `doc-hub`

> **Not part of the protocol.** These are the commands of the reference implementation: your own implementation may build its interface differently, see [protocol](protocol.md). The first version is implemented in the `cli` repository (Python): the agent's working cycle. Of the administrator commands, `init` is implemented; `project`, `area`, `feature`, `agent`, `member` are not, and the configuration is edited by hand. Installation and a reference for the implemented commands are in the `cli` repository.

The executable is `doc-hub`. The version is `version` in `pyproject.toml`: `0.x`, no releases.

- **Hub:** the `--hub` parameter (a path or the `id` of a manifest section), then `DOC_HUB_PATH`, `DOC_HUB_ID`, the first section of `.doc-hub.yaml`, see [protocol](protocol.md#context-resolution).
- **Agent:** `--agent`, `DOC_HUB_AGENT`, then the manifest's `agent` with `DOC_HUB_AGENT_SUFFIX`, see [access](access.md#identity). Without a name the agent gets the name `anonim`: writing is forbidden, and reading is allowed without setup. It can be denied in a project with `projects.<id>.anonim: false` and restricted to paths with `members.anonim.paths`. The name `anonim` is reserved: it is not added to `agents`.
- **Project:** the `--project` parameter, otherwise the first element of `projects`, then `access` of the hub section, otherwise the only project where the agent is a member.

An agent runs in its own project's repository, not in the hub.

## General requirements

- Output is text in English; with `--json`, JSON for agents.
- Exit code `0` means success; non-`0` means an error with a clear message (no permission, no project, wrong format, agent not defined).
- Permissions, path restrictions and schemas are checked.
- Writing commands: `git pull --rebase` → write → `commit` of own files only on behalf of the agent → `push` (if there is an origin), see [hub-storage](hub-storage.md#synchronization-between-machines).
- The CLI does not edit external documentation: `docs sync` only exports it to the hub.

## Hub and projects

| Command | What it does | Who |
|---|---|---|
| `doc-hub init [<path>] [--id …] [--name …] [--agent …] [--project …]` | Creates the hub storage and the first commit; asks for missing parameters in the terminal; does not change an existing hub | administrator |
| `doc-hub project register <id>` | Registers a project | administrator |
| `doc-hub project list` | Projects available to the agent, its roles and paths | everyone |
| `doc-hub project show [<id>]` | Paths, areas, owners, members, features | members |
| `doc-hub area set <path> --owners a,b` | Area and owners | administrator |
| `doc-hub feature enable\|disable <feature>` | Enable / disable a feature | administrator |
| `doc-hub agent add <name> --description …` | Add an agent | administrator |
| `doc-hub member set <agent> --role participant\|reader [--paths …]` / `member remove` | Project member | administrator |
| `doc-hub whoami` | Agent name, hub, projects and roles | everyone |
| `doc-hub validate` | Checks configuration and data by the schemas | everyone |
| `doc-hub install [--hub …] [--project …] [--agent …] [--global\|--dir <folder>] [--force] [--instructions]` | Connects a repository: creates the manifest or a new hub's section, adds the manifest to `.gitignore`, installs the skill, offers a DocHub section in `CLAUDE.md` and `AGENTS.md`. Asks for missing parameters in the terminal. Does not change the hub | everyone |

## Wiki

| Command | What it does | Who |
|---|---|---|
| `doc-hub wiki list [<path>]` | Documents in available paths | everyone |
| `doc-hub wiki read <path\|project:path[@commit]>` | Contents and the hub commit | everyone |
| `doc-hub wiki search <query>` | Plain text search | everyone |
| `doc-hub wiki owner <path>` | Area and owners of a document | everyone |
| `doc-hub wiki write <path> --message … [--file …] [--base <commit>]` | Writes a document; text from a file or stdin; `--base` refuses if the file has changed | area owner |
| `doc-hub wiki move <from> <to> --message …` | Moves | area owner |
| `doc-hub wiki delete <path> --message …` | Deletes | area owner |

## Docs

| Command | What it does |
|---|---|
| `doc-hub docs add [<folder>] [--repository …] [--branch …] [--path …] [--local …] [--no-sync]` | Connects a source: a local folder by default, a repository with `--repository`. Writes it to the hub configuration keeping comments, remembers the path on this machine and exports. `--path` is from the current folder. Asks for missing parameters in the terminal. Project owner only |
| `doc-hub docs remove [<folder>] [--yes]` | Disconnects a source: removes it from the hub configuration and deletes the export, without changing the original files. Project owner only |
| `doc-hub docs sync [<folder>…] [--ref <ref>]` | Exports sources, see [docs-sources](docs-sources.md). Does not commit to the hub's git |
| `doc-hub docs status [<folder>…]` | Export commit, time, lag, unpushed commit, uncommitted changes |
| `doc-hub docs list [<path>]` | Documents in available paths |
| `doc-hub docs read <path\|project:path[@commit]>` | Contents: from the export, otherwise from the clone; the output includes the commit |
| `doc-hub docs search <query>` | Plain text search |

`docs sync`, `docs status` and `status` show the warnings from [docs-sources](docs-sources.md#updating).

## Discussions

| Command | Who |
|---|---|
| `doc-hub discussion list [--status …] [--kind …] [--to <agent>] [--ref <path>] [--mine]` | everyone with access |
| `doc-hub discussion show <discussion>` | everyone with access |
| `doc-hub discussion create --kind question\|proposal\|note --title … [--to a,b] [--ref project:path] [--slug …] --message <text\|->` | `participant` |
| `doc-hub discussion reply <discussion> --message <text\|->` | `participant` |
| `doc-hub discussion answered <discussion> --outcome … [--message …]` | author, area owner |
| `doc-hub discussion accept\|reject <discussion> --outcome … [--message …]` | area owner |
| `doc-hub discussion apply <discussion> --changes path,… --commit <sha> [--outcome …] [--message …]` | area owner |
| `doc-hub discussion close <discussion> --outcome … [--message …]` | author, addressee (`note`), area owner |
| `doc-hub discussion search <query>` | everyone with access: search by outcomes and topics |
| `doc-hub inbox`: discussions that need a reaction, see [inbox](features/discussions.md#inbox) | everyone |

`<discussion>` is a file name without the extension (`2026-11-06-error-format`) or a `uid`. `--message -` reads text from stdin.

`doc-hub` does not check the queue, the round limit and rollback yet: at level 2 they rest on the rules.

`discussion show` warns if a document has changed after the commit from `refs`. `discussion create` warns if a commit from `refs` is not pushed to origin.

## Changes

| Command | What it does |
|---|---|
| `doc-hub changes [--since <sha>] [--diff]` | Wiki and docs changes after the agent's mark |
| `doc-hub changes ack` | Updates the view mark: wiki and all docs |

## Claims

| Command | What it does |
|---|---|
| `doc-hub claim add <paths…> --topic … [--for 4h] [--discussion …] [--slug …]` | Sets a claim; on overlap, a warning and refusal without `--force` |
| `doc-hub claim list [--path …]` | Active claims |
| `doc-hub claim check <paths…>` | Whether there are overlaps |
| `doc-hub claim extend <claim> --for 2h` / `claim release <claim>` | Extend / release |

## Board

| Command | Who |
|---|---|
| `doc-hub card list [--status …] [--assignee <agent>] [--ref <path>] [--mine]` | everyone with access |
| `doc-hub card show <card>` | everyone with access |
| `doc-hub card add --title … [--status backlog\|todo] [--assignee …] [--priority low\|normal\|high] [--due YYYY-MM-DD] [--ref project:path] [--discussion …] [--slug …] [--message <text\|->]` | `participant` |
| `doc-hub card edit <card> [--title …] [--assignee …] [--priority …] [--due …] [--ref …] [--discussion …] [--message <text\|->]` | author, assignee, project owner |
| `doc-hub card take <card>` | see [statuses](features/board.md#statuses) |
| `doc-hub card move <card> todo\|doing\|done\|closed [--outcome …]` | see [statuses](features/board.md#statuses) |
| `doc-hub card close <card> --outcome …` | author, project owner |

`<card>` is a file name without the extension or a `uid`. `--message` sets the card description. An empty value in `card edit` clears the field.

## Session start summary

`doc-hub status` is one command for an agent at session start: who I am, the inbox (`inbox`), changes since the last view, active claims for the project, own unfinished cards, docs export lag.

## Default values

| What | Value |
|---|---|
| Claim duration | 4 hours (*proposal*) |
| First view of changes, without a mark | last 20 commits on available paths (*proposal*) |
| Retries of a discussion write on collision | up to three attempts |
