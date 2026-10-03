---
name: adr-0007-engine-content-separation
description: Source stub — engine ADR-0007, the engine holds no knowledge and content lives in per-domain libraries.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-10-02
tags: [knowledge-architecture, source]
keywords: [engine, library, separation, repository kinds, cross-repository citation, ADR]
---
# Source: engine ADR-0007, the engine holds no knowledge and content lives in per-domain libraries

- **Title:** ADR-0007: The engine holds no knowledge; content lives in per-domain library repositories
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** 2026-09-19
- **Date ingested:** 2026-10-02
- **Artifact location:** `ai-kb:documents/decisions/0007-engine-content-separation.md`, read at engine commit `35f0e95` on 2026-10-02. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.

The engine's decision record on splitting behaviour from content. Folded from it: the reason for the split, what a library is, why there is no router chain or registry, and the cost of an unguarded library cloned alone. Its cross-repository citation rule is already stated in the contract (`ai-kb:CONVENTIONS.md` §6) and was linked, not restated.
