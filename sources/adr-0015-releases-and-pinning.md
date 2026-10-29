---
name: adr-0015-releases-and-pinning
description: Source stub — engine ADR-0015, what a release is, and how libraries pin the engine.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-10-28
tags: [knowledge-architecture, source]
keywords: [release, semantic versioning, freeze, code freeze, pinning, library release, CHANGELOG]
---
# Source: ADR-0015: What a release is, and how libraries pin the engine

- **Title:** ADR-0015: What a release is, and how libraries pin the engine
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** 2026-10-16 (amended 2026-10-21 and 2026-10-23)
- **Date ingested:** 2026-10-28
- **Artifact location:** `ai-kb:documents/decisions/0015-releases-and-pinning.md`, read at engine commit `c9dc309` on 2026-10-28. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.
- **Re-read:** at engine commit `d16b78c` on 2026-10-28, with the engine's `main` at that commit: only its status line changed, and now names the 2026-10-21 and 2026-10-23 amendments. Nothing folded from it changed.

The engine's decision record on what a release is. Folded from it: semantic versioning of the engine, why the first release was not 0.1.0, the two-day freeze and what merges in it, and what a library release records. Its pinning section is superseded by ADR-0016 and is folded only as that supersession. Not folded: the release date, which is a schedule, and the delivery detail of the first release.
