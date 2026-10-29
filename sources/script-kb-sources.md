---
name: script-kb-sources
description: Source stub — header of the engine's kb-sources script, external Source URL liveness.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-10-16
tags: [knowledge-architecture, source]
keywords: [kb-sources, URL liveness, provenance rots, source-dead, source-unreachable, --yes]
---
# Source: header of the engine's kb-sources script, external Source URL liveness

- **Title:** kb-sources (script header)
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** undated in the artifact
- **Date ingested:** 2026-10-16
- **Artifact location:** `ai-kb:scripts/kb-sources` (docstring), read at engine commit `c9dc309` on 2026-10-28. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.

Source for the provenance-liveness check as built. Folded from it: its three rules and that a dead or unreachable URL is queued, never edited. Not folded: the classification table and queue-record fields. Re-read 2026-10-28 for the request sequence: HEAD, then GET on any HTTP error but 429 with GET deciding, then one more GET with the default client name on a 403.
