---
name: script-kb-eval-sample
description: Source stub — header of the engine's kb-eval-sample script, a graded retrieval sample joined with its cost.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-10-28
tags: [knowledge-architecture, source]
keywords: [kb-eval-sample, seeded sample, grade-routing, grade-eval, cost, --cases]
---
# Source: kb-eval-sample (script header)

- **Title:** kb-eval-sample (script header)
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** undated in the artifact
- **Date ingested:** 2026-10-28
- **Artifact location:** `ai-kb:scripts/kb-eval-sample` (docstring), read at engine commit `c9dc309` on 2026-10-28. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.

Source for how a seeded sample of golden cases is prepared, put to fresh agents and graded by the two existing graders. Folded from it: the two steps, why `--cases` exists, and that the runs are kept out of normal-use cost. Not folded: the options and exit codes.
