---
name: script-kb-prepare
description: Source stub — header of the engine's kb-prepare script, unattended preparation of a review packet.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-10-28
tags: [knowledge-architecture, source]
keywords: [kb-prepare, review packet, headless, Read Grep Glob, claude -p, budget, redact]
---
# Source: kb-prepare (script header)

- **Title:** kb-prepare (script header)
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** undated in the artifact
- **Date ingested:** 2026-10-28
- **Artifact location:** `ai-kb:scripts/kb-prepare` (docstring), read at engine commit `c9dc309` on 2026-10-28. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.

Source for how preparation runs: only when due, with a read-only tool list and a dollar cap, the header written by the script and the body filtered by the redaction filter, the packet saved outside the library. Folded from it: those rules and the exit codes' meaning. Not folded: the prompt text and the packet layout.
