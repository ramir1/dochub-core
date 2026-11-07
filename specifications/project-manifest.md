---
type: doc
lang: en
source: project-manifest.ru.md
source_updated: 2026-11-06
status: draft
updated: 2026-11-07
---

# The `.doc-hub.yaml` manifest

`.doc-hub.yaml` lies in the repository root and tells agents working there **which hubs** to connect to, and for each hub **which of its projects** belong to this repository or are needed by it, and **which agent name** to use by default.

The manifest is **separate on each machine and not stored in git**: it holds paths to hub clones and agent names. The format is YAML, so comments can be left in the file. The file is added to the repository's `.gitignore`. Schema: [schemas/project-manifest](../schemas/project-manifest.md), template: [templates/doc-hub.yaml](../templates/doc-hub.yaml).

What the manifest does **not** contain:

- **permissions**: the hub grants them;
- **secrets**.

## Examples

The portal repository is the `portal` project itself:

```yaml
hubs:
  main:
    path: ~/projects/hub
    projects: [portal]
    agent: portal
```

A repository that is not registered in the hub but works with one of its projects has `projects: []` and `access: ["portal"]`, see [contract-zone](../examples/contract-zone.md#mobile-app-repository).

## Several hubs

Parameters are split by hub: the section key is `hub.id` from the hub configuration. A repository can work with several hubs:

```yaml
hubs:
  main:
    path: ~/projects/hub
    projects: [portal]
    agent: portal
  work:             # the customer's hub
    path: ~/work/hub
    projects: []
    access: [api]
    agent: portal-client
```

An implementation appends a new section to the end of the file and does not re-save it, so as not to lose comments: `hubs` must remain the last section of the file. The hub is chosen in the order given in the [protocol](protocol.md#context-resolution), by default the first section. Projects and the agent name come from the section of the chosen hub.

## Agent name

`agent` is the default name in this hub. Another agent in this repository is given a suffix or a full name, see [access](access.md#identity).

## Hub location

`path` is the path to the local hub clone; `url` is a service address: the field is reserved for level 3 implementations, and the implementation defines its meaning. The path depends on the machine, so it is overridden by the `DOC_HUB_PATH` environment variable; the variable takes priority.
