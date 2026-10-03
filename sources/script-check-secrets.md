---
name: script-check-secrets
description: Source stub — header and output text of the engine's check-secrets script, the three-pass secret and identifier scan.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-10-02
tags: [knowledge-architecture, source]
keywords: [check-secrets, secret scan, three passes, lint-allow, PII, worktree, --file, enforcement boundary]
---
# Source: header and output text of the engine's check-secrets script, the three-pass scan

- **Title:** check-secrets (script header and closing advice)
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** undated in the artifact
- **Date ingested:** 2026-10-02
- **Artifact location:** `ai-kb:scripts/check-secrets` (docstring and closing output text), read at engine commit `35f0e95` on 2026-10-02. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.

Source for what the scan covers, its modes, its stated limits, and the advice it prints. Folded from it: the three passes, that any pass blocks, the `lint-allow` escape, the shared allowlist, the four modes, the limits and the boundary statement. Not folded: the name of the hosting platform's server-side feature the header cites (product behaviour), and the scan's patterns file, which is not read here.
