---
name: adr-0010-cross-library-links-checked-at-commit
description: Source stub — engine ADR-0010, cross-library links resolved at every commit that can break one, not in CI.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-11-20
tags: [knowledge-architecture, source]
keywords: [ADR-0010, check-xlinks, cross-library link, commit hook, kb-audit, credential, siblings]
---
# Source: ADR-0010: Cross-library links are resolved at every commit that can break one, not in CI

- **Title:** ADR-0010: Cross-library links are resolved at every commit that can break one, not in CI
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** 2026-09-24
- **Date ingested:** 2026-11-20
- **Artifact location:** `ai-kb:documents/decisions/0010-cross-library-links-checked-at-commit.md`, read at engine commit `5433f08` on 2026-11-20. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.

The engine's decision on where `check-xlinks` runs. Folded from it, in [[contract-layout-and-naming]]: the gap, the three options, the decision and how the library hook decides it can check, what it accepts as incomplete, and consequences. Not folded: the deciders line.
