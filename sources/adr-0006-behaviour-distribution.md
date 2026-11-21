---
name: adr-0006-behaviour-distribution
description: Source stub — engine decision: behavior travels as a plugin registered explicitly, from a directory source.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-11-20
tags: [knowledge-architecture, source]
keywords: [ADR-0006, plugin, marketplace, extraKnownMarketplaces, enabledPlugins, directory source, shadow, restart]
---
# Source: ADR-0006, the knowledge base is its own repository and its behaviour travels as a plugin

- **Title:** ADR-0006, the knowledge base is its own repository and its behaviour travels as a plugin
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** undated in the artifact
- **Date ingested:** 2026-11-20
- **Artifact location:** `ai-kb:documents/decisions/0006-behaviour-distribution.md`, read at engine commit `5433f08` on 2026-11-20. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.

Engine decision record, accepted 2026-09-19 and amended the same day, and amended by ADR-0007. Folded from it: the registration settings, why registration is explicit, why the directory source stays, the correction that behavior is loaded once at session start, the shadowing hazard and the review triggers. Not folded: the table of files that moved in and the check-kb exclusion list, which ADR-0007 reshaped, and the remote and CI details.
