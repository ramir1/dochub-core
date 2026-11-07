---
type: doc
lang: en
source: integration.ru.md
source_updated: 2026-11-07
status: draft
updated: 2026-11-07
---

# Connecting to DocHub

How to create a hub and connect a project to it. There are no commands here: the chosen implementation provides them. Concepts: [glossary](../concepts/glossary.md), levels: [levels](../concepts/levels.md).

## Order

1. Choose a work level.
2. Create a hub if there is none yet.
3. Register the project and agents.
4. Connect external documentation if it is needed.
5. Connect the project repository.
6. Connect people and other machines.
7. Check.

## 1. Level

| Level | What is needed | When it fits |
|---|---|---|
| 1. Core | Core and a hub folder; the agent works by the [dochub](../skills/dochub/SKILL.md) skill | Trying it out, no implementation |
| 2. Utility | An implementation on each agent machine | Ongoing work: the utility checks permissions and formats |

The reference implementation is the `cli` repository; its README describes installation and setup. You can move from level 1 to level 2 later: the file format is the same.

## 2. Hub

One hub usually serves all projects. The hub administrator creates it; at level 2 an implementation usually does these steps:

1. Create a git repository. Several machines need a shared origin, for one it is optional, see [hub-storage](../specifications/hub-storage.md#synchronization-between-machines).
2. Copy the templates: [hub-gitignore](../templates/hub-gitignore) → `.gitignore`, [hub-AGENTS](../templates/hub-AGENTS.md) → `AGENTS.md` and its [Russian version](../templates/hub-AGENTS.ru.md) → `AGENTS.ru.md`, [hub-obsidian](../templates/hub-obsidian.md) → `OBSIDIAN.md` and its [Russian version](../templates/hub-obsidian.ru.md) → `OBSIDIAN.ru.md`. Add a `README.md` with a description of the hub.
3. Create `.doc-hub/config.yaml` with `format_version`, `hub`, `agents` and `projects`, see [hub-config](../schemas/hub-config.md).
4. Commit and push to origin.

Structure: [hub-storage](../specifications/hub-storage.md#structure). Feature folders appear on the first write.

## 3. Project and agents

In `.doc-hub/config.yaml`, by the [hub-config](../schemas/hub-config.md) schema:

1. **Agents:** each one in `agents`. Names follow [naming](naming.md#agents): different sessions and tools are different agents.
2. **Project:** an entry in `projects.<id>`: `name`, `owners`, `members`.
3. **Members:** in `members`: the role and, if needed, `paths`, see [access](../specifications/access.md). Owners must be `participant`.
4. **Areas:** `areas`, if parts of the documentation have different owners, see [projects](../specifications/projects.md#areas).
5. **Features:** only the needed ones, in `features`, see [features](../concepts/features.md).

A project's own documentation lives in `projects/<id>/wiki/`, its requirements are in [default-rules](default-rules.md#wiki-requirements). An agent of another project that needs a contract is given membership with `paths`: for an example see [contract-zone](../examples/contract-zone.md).

## 4. External documentation

Optional. What to keep in the repository and what in the wiki: see [wiki](../specifications/wiki.md#what-goes-in-the-wiki). At level 2 an implementation usually does steps 1–3.

1. Describe the source in `projects.<id>.docs.<folder>`, see [docs-sources](../specifications/docs-sources.md#connecting).
2. On each machine, set the root in `.doc-hub/config.local.yaml`: the project root or clone. This file does not reach git.
3. Run an export with the implementation. At level 1 there is no implementation: the agent reads the documentation from the folder or clone.

## 5. Project repository

In the repository where agents work:

At level 2 an implementation usually does steps 1, 2 and 5.

1. **Manifest.** Put `.doc-hub.yaml` from the [doc-hub.yaml](../templates/doc-hub.yaml) template in the root (with Russian comments: [doc-hub.ru.yaml](../templates/doc-hub.ru.yaml)) and add it to the repository's `.gitignore`: the file is separate on each machine, see [project-manifest](../specifications/project-manifest.md).
2. **Instructions.** Add the text from the [project-agents-snippet](../templates/project-agents-snippet.md) template to `CLAUDE.md` or `AGENTS.md`. Without it the agent will not learn about the hub.
3. **Agent name.** Set `agent` in the manifest's hub section. For another agent in this repository, set `DOC_HUB_AGENT_SUFFIX` in the tool or session environment: it does not need to be registered in the hub, see [access](../specifications/access.md#suffix).
4. **Several hubs.** Add a section per hub to the manifest; the first section is used by default, see [project-manifest](../specifications/project-manifest.md#several-hubs).
5. **Skill.** At level 1 connect the [dochub](../skills/dochub/SKILL.md) skill in the way the tool expects. At level 2 install the implementation and connect its instructions for agents.

## 6. People and other machines

- **People** work in Obsidian: for setup see [obsidian](obsidian.md).
- **A new machine:** clone the hub, create `.doc-hub/config.local.yaml` if needed, install the implementation at level 2, and create a separate manifest in each repository.
- **A new agent:** with its own permissions, add it to `agents` and `members`; with the base agent's permissions, just set a suffix.

## 7. Checking

1. Run data validation with the implementation, see [protocol](../specifications/protocol.md#data-validation).
2. Start an agent in the project repository: it should determine its name, hub, project and role and show the session start summary, see [rules/agents](agents.md#session-start).
3. Create a test `note` discussion for another agent and make sure it appears in that agent's inbox.
