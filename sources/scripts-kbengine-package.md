---
name: scripts-kbengine-package
description: Source stub — header of the engine's the kbengine package, the shared readers every engine script imports.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-11-20
tags: [knowledge-architecture, source]
keywords: [kbengine, attention, clock, contract, env, factfiles, library, queue, registry]
---
# Source: module docstrings of the engine's the kbengine package

- **Title:** the kbengine package (module docstrings)
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** undated in the artifact
- **Date ingested:** 2026-11-20
- **Artifact location:** `ai-kb:scripts/kbengine/` (module docstrings), read at engine commit `5433f08` on 2026-11-20. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.
- **Re-read:** on 2026-11-24 at engine commit `303082a`, the module docstrings. The package gained `lease`, `lint`, `golden`, `citations`, `transcripts` and `reports`; `attention` gained `items`; `factfiles` gained `tree`, `tree_from`, `FENCE`, `EXCLUDED_DIRS` and `served`; `library` reads `unasserted_ttl`; `contract` gained `rules_version`; `env` gained `nudge_override`.

Source for the script reference ([[script-reference-tools]]). Folded from it: each module's docstring purpose and the fact that scripts import the package as a sibling. The function lists inside docstrings were not folded. Not folded: implementation detail beyond the header.
