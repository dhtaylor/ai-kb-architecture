---
name: adr-0011-no-orchestrator-until-routing-fails
description: Source stub — engine ADR-0011, no orchestrator is built until retrieval's own routing is shown to fall short (as amended 2026-10-12).
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-10-02
tags: [knowledge-architecture, source]
keywords: [orchestrator, routing, fan-out, kb-retrieve, revisit trigger, ADR]
---
# Source: engine ADR-0011, no orchestrator is built until retrieval's own routing is shown to fall short

- **Title:** ADR-0011: No orchestrator is built until retrieval's own routing is shown to fall short
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** 2026-09-28, amended 2026-10-12
- **Date ingested:** 2026-10-02
- **Artifact location:** `ai-kb:documents/decisions/0011-no-orchestrator-until-routing-fails.md`, read at engine commit `35f0e95` on 2026-10-02, and re-read with its 2026-10-12 amendment at engine commit `7541729`. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.
- **Re-read:** at engine commit `720fcff` on 2026-10-16, for the library-discovery sentence, which ADR-0014 superseded.
- **Re-read:** at engine commit `d16b78c` on 2026-10-28, with the engine's `main` at that commit: unchanged since its 2026-10-12 amendment. Nothing folded from it changed.

The engine's decision record not to build the orchestrator the plan specified. Folded from it: why one retrieval agent already does the routing, the 12-question sample and its results, the three revisit triggers, and the stated limits of the evidence; and, from the 2026-10-12 amendment, that two of the three agents read all six routers and the third read only three. The sample's own evidence directory is in the engine and was not read in this fold.
