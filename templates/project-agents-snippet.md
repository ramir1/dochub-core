## DocHub

This project is connected to DocHub: agents coordinate work around documentation through a shared hub.

- The agent name and hub are in `.doc-hub.yaml` or in `DOC_HUB_AGENT`, `DOC_HUB_AGENT_SUFFIX`, `DOC_HUB_PATH`. If they are not set, ask a person.
- Work with the hub through the utility if it is installed; otherwise by the `dochub` skill from DocHub Core.
- At session start, look at the summary and react to the inbox.
- Edit the wiki only in your area; for someone else's area, open a `proposal` discussion with its owner.
- If you changed something that affects other agents, open a `note` discussion.
- If you found a problem in the documentation, record it in the hub, not only in the chat.
- At session end, update the view mark and release your claims.

<!-- DocHub Core template: add this section to CLAUDE.md or AGENTS.md of the project repository. -->
