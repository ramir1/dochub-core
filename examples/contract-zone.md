---
type: doc
lang: en
source: contract-zone.ru.md
source_updated: 2026-11-06
status: draft
updated: 2026-11-07
---

# Example: a cross-project contract as an area

Case B from [use-cases](../concepts/use-cases.md#b-cross-project-contract). The documentation "Portal external perimeter for the mobile app" is stored in the portal project's wiki, in `wiki/mobile-app/`, and is edited by its owner, the backend agent.

## Hub configuration (fragment)

```yaml
projects:
  portal:
    owners: [ramir]
    areas:
      wiki/mobile-app: { owners: [ramir-claude-backend] }
    members:
      ramir:                { role: participant }
      ramir-claude-backend: { role: participant }
      mobile-dev:           { role: participant, paths: [wiki/mobile-app] }
    features:
      discussions: { enabled: true }
      changes:     { enabled: true }
```

`mobile-dev` is a member of the `portal` project but sees only `wiki/mobile-app`: documentation and discussions of other areas are not available to it.

## Mobile app repository

`.doc-hub.yaml`:

```yaml
hubs:
  main:
    path: ~/projects/hub
    projects: []
    access: [portal]
    agent: mobile-dev
```

**Access to the contract text.** The contract arrives with the hub clone: `mobile-dev` reads it through an implementation, and access to the portal code is not needed. The hub clone shows the whole wiki: the `paths` restriction works in the implementation, see [wiki](../specifications/wiki.md#visibility).

## Scenario

1. `mobile-dev` reads `portal:wiki/mobile-app/09-comments.md` and sees that comments are paginated differently from the rest of the content.
2. It creates a `proposal` discussion "Cursor pagination for comments", `refs: [portal:wiki/mobile-app/09-comments.md]`.
3. `ramir-claude-backend` (the area owner) checks in the portal code whether this is possible, replies and accepts: `accepted` with an outcome.
4. After the API is updated, it edits `wiki/mobile-app/09-comments.md` and marks the discussion `applied` with `changes` and the hub commit.
5. At the start of its next session `mobile-dev` views the changes, sees the document edit and the linked `applied` discussion, and moves to the new pagination.

The recipient does not edit the contract: a change goes through agreement with the contract owner project.
