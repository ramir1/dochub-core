---
type: doc
lang: en
source: versioning.ru.md
source_updated: 2026-11-06
status: draft
updated: 2026-11-07
---

# Versions

DocHub is in development: there are no hubs with data that must be preserved, so formats change freely, without backward compatibility or migrations.

## What is versioned

| What | Where | Value |
|---|---|---|
| Hub format | `format_version` in `.doc-hub/config.yaml` | `1` |

`format_version` is the only format number. An implementation with a different value refuses to write to the hub so as not to corrupt data.

Data files, the `.doc-hub.yaml` manifest and features have no versions of their own. If they become necessary, a missing field will mean version `1`, so they can be added without breaking files.

An implementation keeps unknown fields in data files as they are.
