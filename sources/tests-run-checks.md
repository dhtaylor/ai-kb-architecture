---
name: tests-run-checks
description: Source stub — the engine's regression suite, its environment preamble.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-10-28
tags: [knowledge-architecture, source]
keywords: [run-checks, regression suite, GIT_DIR, GIT_INDEX_FILE, local-env-vars, linked worktree, fixture]
---
# Source: tests/run-checks (preamble comments)

- **Title:** tests/run-checks (preamble comments)
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** undated in the artifact
- **Date ingested:** 2026-10-28
- **Artifact location:** `ai-kb:tests/run-checks` (the comments at the top of the file), read at engine commit `c9dc309` on 2026-10-28. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.

Source for why the suite clears git's repository environment and the other per-user variables before it builds fixtures. Folded from it: the cause and what the clearing prevents. The observed damage is from the issue, which has its own stub.
