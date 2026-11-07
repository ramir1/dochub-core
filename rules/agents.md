---
type: doc
lang: en
source: agents.ru.md
source_updated: 2026-11-06
status: draft
updated: 2026-11-07
---

# Rules for AI agents

An agent works **in its own project's repository**, not in the hub. How it accesses the hub depends on the [level](../concepts/levels.md):

- **there is a utility** (level 2): only through it, without writing hub files by hand. Operations are named below, and commands are in the utility's documentation; for the reference implementation, see [cli-contract](../specifications/cli-contract.md);
- **there is no utility** (level 1): the same actions directly with files by the [dochub](../skills/dochub/SKILL.md) skill.

So that an agent knows about the hub, the text from the [project-agents-snippet](../templates/project-agents-snippet.md) template is added to the project instructions (`CLAUDE.md`, `AGENTS.md`).

## Session start

1. **The name and hub** are set in `.doc-hub.yaml` or in the environment: `DOC_HUB_AGENT`, `DOC_HUB_AGENT_SUFFIX`, `DOC_HUB_PATH`, `DOC_HUB_ID`. If something is missing, ask a person. Do not write to the hub without a name.
2. **Session start summary**: role, paths, inbox, changes, claims, docs state.
3. **React to the inbox:** read the discussions and reply.
4. **If docs lag behind and you need fresh documentation**, run an export if you have repository access.

## During work

- **Before working with a document**, look at discussions with it in `refs`: past decisions and open questions.
- **Before a noticeable edit**, check overlaps with claims, then set your own. A short edit does not need a claim.
- **Edit the wiki** only in your area: check the document's owner first. Read the document before writing and write the edit on top of the version you read. For someone else's area, send a `proposal` to its owner.
- **Docs** are not edited through the hub: only in the source repository, by its rules.
- **If you changed something that affects others**, send a `note` to the addressees.
- **A question to another agent** is a `question` with an addressee, not a request for a person to pass it on.
- **If you found a problem in the documentation**, record it in the hub right away (`question` or `proposal`). In the chat with a person, mention only what only they can solve: other agents do not see the chat.
- Documentation rules: [default-rules](default-rules.md).

## Session end

- Release or extend your claims.
- Update the view mark.
- Outcomes of agreements go in the `outcome` of discussions, not only in your tool's memory.

## Prohibitions

- Do not edit other agents' wiki areas or export files in `projects/<id>/docs/`.
- Do not read project paths outside your `paths`, including directly in the clone.
- At level 2 do not change hub files by hand. At any level do not change `.doc-hub/config.yaml` and `.obsidian/` unless a person asks.
- Do not present assumptions as facts, see [writing](writing.md).
