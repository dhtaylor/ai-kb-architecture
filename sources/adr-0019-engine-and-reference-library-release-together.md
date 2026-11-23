---
name: adr-0019-engine-and-reference-library-release-together
description: Source stub — engine ADR-0019, the engine and its reference library release together, under one tag.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-11-06
tags: [knowledge-architecture, source]
keywords: [release tag, reference library, release package, same tag, release day, v0.4.0]
---
# Source: ADR-0019: The engine and its reference library release together, under one tag

- **Title:** ADR-0019: The engine and its reference library release together, under one tag
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** 2026-11-06
- **Date ingested:** 2026-11-06
- **Artifact location:** `ai-kb:documents/decisions/0019-engine-and-reference-library-release-together.md`, read at engine commit `e2a2851` on 2026-11-06. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.
- **Re-read:** on 2026-11-23 at tag `v0.5.0` (`3b60c08`). Since `e2a2851` the release table gained the v0.4.0 and v0.5.0 rows, and the notes bullet now says ADR-0016 requires a library's notes to name only the contract, and naming the engine too is this library's rule (it said "as ADR-0015 and ADR-0016 require").

The engine's decision record making this library take the engine's release tag. Folded from it: why this library alone is tied to the engine, the shared tag and its first use, what happens to published tags, what the library's notes still record, library-only fixes, the release-day steps, and the stated costs. Not folded: the owner's other libraries having no release cycle, which is a fact about one maintainer's machine, not about the engine.
