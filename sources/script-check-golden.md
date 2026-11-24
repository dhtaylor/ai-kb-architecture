---
name: script-check-golden
description: Source stub — header of the engine's check-golden, the deterministic half of the retrieval eval.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-11-20
tags: [knowledge-architecture, source]
keywords: [check-golden, golden set, oracle, reachable, expected_file]
---
# Source: docstring of the engine's check-golden

- **Title:** check-golden (docstring)
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** undated in the artifact
- **Date ingested:** 2026-11-20
- **Artifact location:** `ai-kb:scripts/check-golden` (docstring), read at engine commit `5433f08` on 2026-11-20. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.
- **Re-read:** on 2026-11-24 at engine commit `303082a`, the docstring and the diff since v0.5.0. Folded from it: the record rules moved into `kbengine.golden`, and `check-golden` checks an `expected_evidence:` value against the file. The usage line and exit codes are unchanged.

Source for the script reference ([[script-reference-checks]]). Folded from it: its purpose, what it verifies, the usage line and the stated exit codes. A usage error exiting 2 was observed, not stated. Not folded: implementation detail beyond the header.
