---
name: script-kb-backup
description: Source stub — header of the engine's kb-backup script, verified bundles of the engine and every library.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-10-28
tags: [knowledge-architecture, source]
keywords: [kb-backup, git bundle, restore test, SHA256SUMS, fsck]
---
# Source: kb-backup (script header)

- **Title:** kb-backup (script header)
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** undated in the artifact
- **Date ingested:** 2026-10-28
- **Artifact location:** `ai-kb:scripts/kb-backup` (docstring), read at engine commit `c9dc309` on 2026-10-28. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.

Source for why a bundle is verified by restoring it and not by git's own bundle check, and what the command refuses. Folded from it: the verification method and the refusals. Not folded: the options and the run directory naming.
