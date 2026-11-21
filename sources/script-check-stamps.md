---
name: script-check-stamps
description: Source stub — header of the engine's check-stamps, the guard against a commit that claims human verification while announcing an agent wrote it.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-11-20
tags: [knowledge-architecture, source]
keywords: [check-stamps, by: human, co-authored-by, range, stamp]
---
# Source: comment block of the engine's check-stamps

- **Title:** check-stamps (comment block)
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** undated in the artifact
- **Date ingested:** 2026-11-20
- **Artifact location:** `ai-kb:scripts/check-stamps` (comment block), read at engine commit `5433f08` on 2026-11-20. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.

Source for the script reference ([[script-reference-checks]]). Folded from it: its purpose, the limit, the usage line and default range, and the exit codes. Exit 2 for a non-repository was observed. Not folded: implementation detail beyond the header.
