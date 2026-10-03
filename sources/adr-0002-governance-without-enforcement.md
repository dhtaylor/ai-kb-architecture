---
name: adr-0002-governance-without-enforcement
description: Source stub — engine ADR-0002, governance structure without enforcement (as amended 2026-09-22 and 2026-10-02).
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-10-02
tags: [knowledge-architecture, source]
keywords: [governance, enforcement, CODEOWNERS, dormant control, ADR]
---
# Source: engine ADR-0002, governance structure without enforcement (as amended 2026-09-22 and 2026-10-02)

- **Title:** ADR-0002: Governance structure without enforcement
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** 2026-09-19, amended 2026-09-22 and 2026-10-02
- **Date ingested:** 2026-10-02
- **Artifact location:** `ai-kb:documents/decisions/0002-governance-without-enforcement.md`, read at engine commit `35f0e95` on 2026-10-02, and re-read with its 2026-10-02 amendment at engine commit `24f5485`. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.

The engine's decision record on governance. Folded from it: the decision driver that a declared-but-unenforced control reads as protection and provides none, and the list of controls that are written but dormant or not in place (CODEOWNERS review, a second owner, signed commits); and, from the 2026-10-02 amendment, that push-time rejection is the boundary only for the shapes the platform recognises, that CI after a push is detection behind it, and that an unenforced repository is recorded as unenforced. Not folded: its statements about hosting-platform plan limits and branch-protection settings, which are product behaviour or deployment configuration, not general design.
