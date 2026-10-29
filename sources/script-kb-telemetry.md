---
name: script-kb-telemetry
description: Source stub — header of the engine's kb-telemetry script, retrieval cost read from transcripts.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-10-28
tags: [knowledge-architecture, source]
keywords: [kb-telemetry, tokens, seconds, steps, wasted, protocol, subagent transcripts, windows]
---
# Source: kb-telemetry (script header)

- **Title:** kb-telemetry (script header)
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** undated in the artifact
- **Date ingested:** 2026-10-28
- **Artifact location:** `ai-kb:scripts/kb-telemetry` (docstring), read at engine commit `c9dc309` on 2026-10-28. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.

Source for the five measures per retrieval run and the choices behind them. Folded from it: what each measure counts and leaves out, how a run is attributed to a library, and that eval runs are left out by default. Not folded: the options.
