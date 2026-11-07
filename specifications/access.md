---
type: doc
lang: en
source: access.ru.md
source_updated: 2026-11-07
status: draft
updated: 2026-11-07
---

# Access: agents, roles, areas

## Agents

An **agent** is a hub member: an AI agent or a person. The name is unique in the hub.

An agent in DocHub is a **working role**, not a program: the same program (Claude Code) in two sessions on different areas is two agents. Name format: [naming](../rules/naming.md#agents).

Agents are listed in the hub configuration (`agents`) with a description: person, tool, purpose.

### Identity

The agent name is not stored in the repository's git: different agents and people work in one repository. By default it comes from the local manifest, see [project-manifest](project-manifest.md).

Resolution order (priority from top to bottom):

1. explicit implementation parameter: the full name;
2. environment variable `DOC_HUB_AGENT`: the full name;
3. the `agent` field of the hub section in the manifest, and if `DOC_HUB_AGENT_SUFFIX` is set, `<agent>-<suffix>`;
4. otherwise the agent is not defined and must not write to the hub. Access without a name is defined by the implementation (levels 2 and 3), for an example see [cli-contract](cli-contract.md). At level 1 the name is requested from a person.

The variables are convenient to set in the tool or session settings. If an agent does not know its name, it should ask a person.

### Suffix

When several agents work in one project, they are given a suffix: `portal-tests` is an agent named `portal-tests`, and `portal` is its **base agent**. The suffix follows the pattern `^[a-z0-9][a-z0-9-]*$`.

A suffixed agent does not need to be registered in the hub: if there is no exact entry, the longest entry `X` for which the name starts with `X-` is used. The rule works on the name itself, so the result is the same on all machines.

- **Inherited:** role, `paths` and ownership of areas and the project.
- **Not inherited:** authorship, addressing, claims and view marks: they go by the full name.
- **An exact entry takes priority:** a registered `portal-tests` lives by its own entry and does not get the permissions of `portal`. An entry is a line in `agents` or in the project `members`, so a registered agent missing from `members` has no permissions in that project.

The rule applies to any names: `ramir-anything` without its own entry gets the permissions of `ramir`, and a typo in a suffix creates another agent, not an error.

**Authentication** is outside Core: the hub trusts the name.

## Roles

A role is given to an agent **in a project**: `projects.<id>.members.<agent>.role`. An agent without an entry in `members` has no access to the project.

| Action | `participant` | `reader` |
|---|:-:|:-:|
| Read documentation (within `paths`) | ✓ | ✓ |
| Read feature data (within `paths`) | ✓ | ✓ |
| Create discussions, write messages | ✓ | — |
| Set and release own claims | ✓ | — |
| Keep own view mark for changes | ✓ | ✓ |
| Decide on proposals (`accept` / `reject` / `apply`) | area owner only | — |

## Path restriction

`members.<agent>.paths` is the list of documentation paths available to the agent. If it is not set, the whole project documentation is available.

```yaml
members:
  mobile-dev: { role: participant, paths: [wiki/mobile-app] }
```

An agent with a restriction sees:

- documents only in its paths;
- discussions that have `refs` all within its paths, as well as those where it is the author or an addressee;
- changes (`changes`) only for its paths;
- claims only for its paths.

## Owners

Owners of an area (`areas.<path>.owners`) or project (`owners`) are agents who decide on proposals and edit documentation in the wiki. An owner must be a `participant` of the project.

## Write policy

1. **The wiki** is written only by the area owner, and outside areas by the project owner: an agent through an implementation or by the skill, a person in Obsidian. **Docs** are not written through the hub; they are edited in their repository.
2. **Feature data:** within the role's permissions, by file see [hub-storage](hub-storage.md#who-changes-which-file).
3. **Hub configuration:** the hub administrator (a person with write access to the hub repository). The model has no separate administrator role.

## Enforcing permissions

At level 1 permissions rest on the rules and the skill, at level 2 the implementation checks them. Real separation is possible only at level 3, see [levels](../concepts/levels.md). An agent that must not see someone else's code should not get a clone of that repository.
