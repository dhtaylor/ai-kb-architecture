---
name: adr-0020-published-libraries-and-consumers
description: Source stub — engine ADR-0020, a library says where the libraries it links to come from, and a consumer's copy is read-only.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-11-09
tags: [knowledge-architecture, source]
keywords: [links_to, dependency mismatch, read-only, consumer, published library, origin, same_origin, kb-library add, --read-only, --ref, aliasing, contract 2.6]
---
# Source: ADR-0020: A library says where the libraries it links to come from, and a consumer's copy is read-only

- **Title:** ADR-0020: A library says where the libraries it links to come from, and a consumer's copy is read-only
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** 2026-11-09
- **Date ingested:** 2026-11-09
- **Artifact location:** `ai-kb:documents/decisions/0020-published-libraries-and-consumers.md`, read at engine commit `5fb833a` on 2026-11-09. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.

The engine's decision record for libraries someone else publishes. Folded from it: the three problems it names, the options weighed, `links_to` and how origins compare, how a mismatch is reported, `kb-library add`'s warnings, the read-only role, the planned pinning, the consequences and what it leaves uncovered. What was built of it is cited from the engine's change notes ([[engine-changelog-v0.4.0]]), not from the decision.
