---
name: adr-0001-kb-root-resolution
description: Source stub — engine ADR-0001, how agents resolve the knowledge root without a literal path; amended by ADR-0008.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-11-20
tags: [knowledge-architecture, source]
keywords: [root resolution, trust root, SessionStart hook, KB_ENGINE_ROOT, settings env, acceptance test, ADR]
---
# Source: engine ADR-0001, knowledge base root resolution

- **Title:** ADR-0001: Knowledge base root resolution
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** 2026-09-19, amended 2026-09-22
- **Date ingested:** 2026-11-20
- **Artifact location:** `ai-kb:documents/decisions/0001-kb-root-resolution.md`, read at engine commit `5433f08` on 2026-11-20. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.

The decision on how an agent finds the root without a path in any file. Folded from it: the chosen mechanism and its 2026-09-22 amendment, the trust-root reason for user-level settings, the acceptance-test findings and the stated costs. Not folded: the test table's checksum and line count, and the open items' present status, which the ADR does not give.
