---
name: script-grade-eval
description: Source stub — header of the engine's grade-eval script, the answer-grounding half of the retrieval eval.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-10-16
tags: [knowledge-architecture, source]
keywords: [grade-eval, grounding, transcripts, SubagentHandback, Sources line]
---
# Source: header of the engine's grade-eval script, the answer-grounding half of the retrieval eval

- **Title:** grade-eval (script header)
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** undated in the artifact
- **Date ingested:** 2026-10-16
- **Artifact location:** `ai-kb:scripts/grade-eval` (docstring), read at engine commit `c9dc309` on 2026-10-28. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.

Source for how answers are graded by grep, and for `--transcripts`: why the answer is read from the hand-back rather than the last text block. Re-read 2026-10-28 for the reader of record values: it imports check-golden's reader, so a quoted excerpt is read as a YAML scalar by both.
