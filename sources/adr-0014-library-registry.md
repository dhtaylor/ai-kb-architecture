---
name: adr-0014-library-registry
description: Source stub — engine ADR-0014, libraries are registered per machine and can live anywhere.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-10-16
tags: [knowledge-architecture, source]
keywords: [registry, library registration, kb-library, kb-libraries, library home, library hook, core.hooksPath, ADR]
---
# Source: engine ADR-0014, libraries are registered per machine and can live anywhere

- **Title:** ADR-0014: Libraries are registered per machine, and can live anywhere
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** 2026-10-15
- **Date ingested:** 2026-10-16
- **Artifact location:** `ai-kb:documents/decisions/0014-library-registry.md`, read at engine commit `720fcff` on 2026-10-16. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.

The engine's decision record replacing discovery under `<engine>/kb/` with a per-machine registry. Folded from it: why a library that disappears failed silently, the registry and where it lives, the reported states, the engine-owned library hook, and its stated consequences. Not folded: the command reference and delivery plan, which are the engine's documentation, and the example registry paths.
