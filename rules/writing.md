---
type: doc
lang: en
source: writing.ru.md
source_updated: 2026-11-06
status: draft
updated: 2026-11-07
---

# Style and reliability

## Reliability

1. **If there is no exact information, say so:** "don't know", "not specified", "not verified". Do not fill gaps with guesses.
2. **Mark the source of confidence.** Marks adopted in project documentation:
   - **"by code"**: derived from source code, not verified on a test stand;
   - **"not confirmed"**: there are doubts;
   - **"proposal"**: not agreed;
   - **"open question"**: not decided.
3. **If behavior differs from a document**, tell the area owner (through a discussion, if the feature is enabled); the area owner fixes the wiki, and docs are fixed in the source repository.

## Contract document header

For a cross-project contract, a header like the one in `mobile-app` is recommended:

```markdown
> **Purpose.** Who this document is for and why.
> **Document version:** 1.4 · **Current as of:** 2026-10-17 · **API:** `v4.1`.
> **How it was assembled.** From which sources; what was verified and what was not.
```

## Style

- Short and to the point. The main thing first, then details.
- One term, one meaning; terms come from the [glossary](../concepts/glossary.md) or the project's glossary.
- Tables for lists with several properties; lists for steps and rules.
- Examples are working ones: real requests, JSON, commands. Mark illustrative data.
- Dates are `YYYY-MM-DD`; date-time is ISO 8601.
- Code, file names, fields, commands go in `monospace`.

## Discussion outcomes

`outcome` is the agents' shared memory. Write it so that an agent who has not read the conversation can understand it: what was decided, where it is reflected, what remains.
