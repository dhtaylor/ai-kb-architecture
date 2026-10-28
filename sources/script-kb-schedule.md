---
name: script-kb-schedule
description: Source stub — header of the engine's kb-schedule script, the opt-in weekly job.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-10-28
tags: [knowledge-architecture, source]
keywords: [kb-schedule, systemd timer, cron, Persistent, lock, weekly job, prepare]
---
# Source: kb-schedule (script header)

- **Title:** kb-schedule (script header)
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** undated in the artifact
- **Date ingested:** 2026-10-28
- **Artifact location:** `ai-kb:scripts/kb-schedule` (docstring), read at engine commit `c9dc309` on 2026-10-28. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.

Source for what the weekly job runs and in what order, that preparation is a second opt-in, and what its log holds. Folded from it: the steps, the opt-in, the lock and the log's content. Not folded: the install options and unit file details.
