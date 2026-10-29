---
name: script-kb-apply
description: Source stub — header of the engine's kb-apply script, the review packets waiting for a person.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-10-28
tags: [knowledge-architecture, source]
keywords: [kb-apply, review packet, STALE, library_head, packets directory]
---
# Source: kb-apply (script header)

- **Title:** kb-apply (script header)
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** undated in the artifact
- **Date ingested:** 2026-10-28
- **Artifact location:** `ai-kb:scripts/kb-apply` (docstring), read at engine commit `c9dc309` on 2026-10-28. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.

Source for how a packet is listed, shown and closed, and when it is stale. Folded from it: that the script never writes to a library and that a stale packet is re-validated by the skill, not discarded. Not folded: the packets directory's layout beyond its being outside every library.
