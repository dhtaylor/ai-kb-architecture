---
name: script-check-kb
description: Source stub — the engine's check-kb script, the contract version gating.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-10-28
tags: [knowledge-architecture, source]
keywords: [check-kb, kb_contract, contract_status, behind, ahead, unsupported, Not covered here]
---
# Source: check-kb (script header and the contract functions)

- **Title:** check-kb (script header and the contract functions)
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** undated in the artifact
- **Date ingested:** 2026-10-28
- **Artifact location:** `ai-kb:scripts/check-kb` (docstring and the functions that read and gate the contract version), read at engine commit `c9dc309` on 2026-10-28. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.

Source for how rules apply by declared contract version, what a library behind or ahead is told, and which majors are supported. Folded from it: the gating rules as the engine implements them, which agree with the contract's section 14. Not folded: the other checks the script makes.
