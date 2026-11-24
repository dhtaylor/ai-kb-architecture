---
name: adr-0016-versioned-library-contract
description: Source stub — engine ADR-0016, the library contract is versioned, and pinning is the library's business.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-10-28
tags: [knowledge-architecture, source]
keywords: [kb_contract, contract version, MAJOR.MINOR, behind, ahead, unsupported, conformance suite, pinning]
---
# Source: ADR-0016: The library contract is versioned, and pinning is the library's business

- **Title:** ADR-0016: The library contract is versioned, and pinning is the library's business
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** 2026-10-21
- **Date ingested:** 2026-10-28
- **Artifact location:** `ai-kb:documents/decisions/0016-versioned-library-contract.md`, read at engine commit `c9dc309` on 2026-10-28. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.
- **Re-read:** on 2026-11-24 at engine commit `324e2ce` (main after #263 and #265), the amendment note dated 2026-11-24 (#263): a library behind on a supported contract is no longer told in the session-start nudge and no longer a decision in the weekly report. Folded into [[releases-and-contract-versioning]].

The engine's decision record giving the library contract its own version. Folded from it: the options weighed, what a minor and a major mean, the declared version and its default, the two-major support window, behind and ahead handling, why pinning is out of the engine's scope, and the stated costs. Not folded: its delivery order, which is the engine's plan. Which delivery steps are built is recorded as an open question.
