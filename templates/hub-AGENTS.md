# DocHub: instructions for AI agents

This is a DocHub hub: a single place for project documentation and for coordinating agents of different tools, sessions and machines.
Copied to the hub root as `AGENTS.md`.

## Working

Agents are not launched in the hub: they work in their own repositories.

**If there is a utility**, work only through it and do not write files by hand. Commands are in its documentation; for the reference implementation, see `specifications/cli-contract` from DocHub Core:

- session start: a summary of who you are, the inbox, changes, claims;
- discussions and the inbox;
- claims and changes;
- own documentation (wiki): write only in your area;
- external documentation (docs): read-only.

**If there is no utility**, work with the files yourself by the skill `skills/dochub/SKILL.md` from DocHub Core.

## Where things are

- `projects/<id>/discussions/`, `changes/`, `claims/`: feature data.
- `.doc-hub/config.yaml`: projects, areas, agents, roles. Do not change.
- `projects/<id>/wiki/`: own documentation.
- `projects/<id>/docs/<folder>/`: external, read-only, not in git.

## Specifications

DocHub Core: `rules/agents`, `specifications/cli-contract`, `specifications/access`, `specifications/features/*`.
<!-- Set the path or link to the DocHub Core used by this hub. -->
