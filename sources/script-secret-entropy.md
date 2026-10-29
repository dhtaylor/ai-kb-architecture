---
name: script-secret-entropy
description: Source stub — header of the engine's secret-entropy script, the heuristic pass for shapeless credentials.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-10-02
tags: [knowledge-architecture, source]
keywords: [secret-entropy, heuristic, high entropy, labelled value, lint-allow]
---
# Source: header of the engine's secret-entropy script

- **Title:** secret-entropy (script docstring)
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** undated in the artifact
- **Date ingested:** 2026-10-02
- **Artifact location:** `ai-kb:scripts/secret-entropy` (docstring), read at engine commit `35f0e95` on 2026-10-02. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.
- **Re-read:** at engine commit `720fcff` on 2026-10-16, for the `--no-bare` flag and the removal of its three-hit cap (engine issue #56).
- **Re-read:** at engine commit `d16b78c` on 2026-10-28, with the engine's `main` at that commit: it gained a `--redact` mode for the redaction filter, which prints only line numbers and lets `lint-allow` exempt nothing. The two rules and the stated limit folded from it are unchanged; the filter is folded from the `check-secrets` stub.

Source for the second check-secrets pass: its two rules, what it deliberately does not flag, and its stated limit. Folded: the two rules and the limit. Not folded: the numeric thresholds beyond those stated in the docstring, and the code itself.
