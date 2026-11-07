---
type: doc
lang: en
source: board-agents.ru.md
source_updated: 2026-11-07
status: draft
updated: 2026-11-07
---

# Example: a task board for agents

Case A from [use-cases](../concepts/use-cases.md#a-several-agents-in-one-project) with the [board](../specifications/features/board.md) feature. A person sets tasks, and agents take them on their own.

## Hub configuration (fragment)

```yaml
projects:
  portal:
    owners: [ramir]
    members:
      ramir:                { role: participant }
      ramir-claude-backend: { role: participant }
    features:
      discussions: { enabled: true }
      claims:      { enabled: true }
      board:       { enabled: true }
```

## Scenario

1. `ramir` creates the card "Describe refresh token renewal" in `todo`, with `refs: [portal:wiki/api/auth.md]`.
2. At session start, `ramir-claude-backend` looks through the cards, takes this one and becomes its assignee: `status: doing`.
3. The edit will take noticeable time, so the agent places a claim on `wiki/api/auth.md`.
4. The agent edits the document, removes the claim and moves the card to `done` with an `outcome`.
5. The edit affects the client, so the agent sends a `note` to the addressees.

The claim has expired and gone, while the card with its outcome stays: a month later it is clear what was done and why.
