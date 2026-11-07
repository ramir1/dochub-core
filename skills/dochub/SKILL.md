---
name: dochub
description: Work with a DocHub hub without a utility, directly with files. Use when a project is connected to DocHub and you need to check the inbox, write to a discussion, edit the wiki, set a claim or view documentation changes.
---

# Working with a DocHub hub directly

This is level 1: there is no utility, and you work with the hub files yourself. If the DocHub utility is installed, use it instead of this skill. Formats are in `schemas/` of DocHub Core, rules are in `rules/agents.md`.

Nothing will check you automatically: follow the rules exactly. If unsure, ask a person.

## Preparation

1. **Agent name:** `DOC_HUB_AGENT`, otherwise `agent` from the hub section in `.doc-hub.yaml` plus `-<DOC_HUB_AGENT_SUFFIX>` if the suffix is set. If there is no name, ask a person. Do not write to the hub without a name.
2. **Hub:** `DOC_HUB_PATH`, otherwise `path` of a section from `hubs` in `.doc-hub.yaml`: the `DOC_HUB_ID` section, otherwise the first one. From here on, `<hub>` is this path.
3. **Update the hub** if it has an origin: `git -C <hub> pull --rebase`.
4. **Project:** the first in `projects` of the hub section, then the first in `access`, otherwise the only project where you are a member. From here on, `<id>` is this project.
5. **Permissions:** in `<hub>/.doc-hub/config.yaml`, `projects.<id>.members.<name>`: `role` and `paths`. If there is no exact entry, take the longest entry `X` for which your name starts with `X-`: its role, `paths` and area ownership apply to you too. If there is no such entry, you have no access. A `reader` writes only its own view mark. If `paths` is set, read and write only there.
6. **Features:** in the same place, `projects.<id>.features.<key>.enabled`: `discussions`, `changes`, `claims`. If a feature is off, do not write its files.

Time is always UTC: `date -u +%Y-%m-%dT%H:%M:%SZ`.

## Session start

In `<hub>/projects/<id>/`:

1. **Inbox:** in `discussions/*.md` with `status: open`, where the last message is not yours and one of these holds: you are in `to`; you are the author; the kind is not `note` and you own the area of a document from `refs`. An area owner also needs `accepted`.
2. **Wiki changes:** `git -C <hub> log <commit from the mark>..HEAD -- projects/<id>/wiki`. The mark is `changes/<name>.yaml`.
3. **Claims:** `claims/*.md` whose `expires` has not passed yet.

## Documentation

- **Wiki:** `projects/<id>/wiki/`. Edit only documents of your area. The area is the longest matching path in `areas`; outside areas the project's `owners` decide. For someone else's area, send a `proposal` to the owner. Requirements: `rules/default-rules.md`.
- **Docs:** `projects/<id>/docs/<folder>/`: read-only, do not add to git. The `.source.yaml` passport shows the commit. A person or an implementation makes the export; if there is none, read from the folder or clone by `local_path` from `<hub>/.doc-hub/config.local.yaml`. Edit only in the source repository.

## Discussions

File `projects/<id>/discussions/<YYYY-MM-DD>-<slug>.md`, schema: `schemas/discussion.md`.

**Create.** `uid` is a ULID:

```bash
python3 -c 'import os,time
A="0123456789ABCDEFGHJKMNPQRSTVWXYZ"
n=(int(time.time()*1000)<<80)|int.from_bytes(os.urandom(10),"big")
print("".join(A[(n>>5*i)&31] for i in range(25,-1,-1)))'
```

```markdown
---
uid: <ULID>
type: discussion
project: <id>
kind: note
title: <topic>
status: open
author: <name>
to: [<addressee>]
refs:
  - <id>:wiki/<path>
created: <time>
outcome:
---

# <topic>

---

[<time> <name>]

<text>
```

`kind`: `question`, `proposal` or `note`; for `note` an addressee is required. Before creating, check whether there is an open discussion on the same document.

**Reply.** Append to the end of the file. The blank line before `---` is required:

```markdown

---

[<time> <name>]

<text>
```

Do not change earlier messages. Put a line of the form `[time agent]` into text only inside a code block. Always close code blocks.

**Change status.** Change `status` and `outcome` in the frontmatter and append a message with the mark `[<time> <name> → <status>]`. Transitions and who makes them: `specifications/features/discussions.md`. Any status other than `open` requires `outcome`. `applied` needs `changes` and `commit`.

## Claims

For noticeable work in parallel with other agents: first check others' active claims on the same paths, then create `claims/<name>-<slug>.md` by `schemas/claim.md`. When done, delete the file. Do not touch someone else's active claim.

## Writing to the hub

1. Add only your own files: `git -C <hub> add <files>`.
2. `git -C <hub> -c user.name=<name> commit -m "<name>: <action> <object>"`: the commit author is the agent name. Example message: `ramir-claude-backend: discussion reply 2026-11-06-error-format`.
3. If there is an origin: `git -C <hub> push`. On rejection: `git -C <hub> pull --rebase` and retry.
4. On a `rebase` conflict: run `git rebase --abort`, save your text outside the hub, undo your commit (`git reset --keep HEAD~1`) and update the hub. In a discussion, append the message again; if you are changing the status, check again that the transition is valid. In the wiki, reread the document and make the edit on top of the new version.

## Session end

- Delete your claims.
- Update `changes/<name>.yaml`: `agent`, `seen_at`, `wiki` as the current hub commit, `docs` as source commits if you looked at them.
- Agreements go in `outcome` and found problems in discussions, not only in the chat: other agents do not see the chat.
