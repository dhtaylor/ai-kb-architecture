---
name: state-change-gates
description: Source stub — engine document classifying every state-changing entry point against the plan's three automation bands.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-10-02
tags: [knowledge-architecture, source]
keywords: [state change, gate, automation band, mutation, check-blast, curation skills]
---
# Source: engine document classifying every state-changing entry point against the plan's three automation bands

- **Title:** State-change gates
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** undated in the artifact
- **Date ingested:** 2026-10-02
- **Artifact location:** `ai-kb:documents/state-change-gates.md`, read at engine commit `35f0e95` on 2026-10-02. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.

The engine's classification of each script, command and skill by what it changes and what gates it. Folded from it: that a skill's stop-for-approval is a request a model honours rather than a wall, that flag-gated scripts are checked by the interpreter, how the high-blast gate behaves on a pull request versus a direct push, and that the 2-reviewer rule is dormant. Not folded: the per-entry-point table, which describes this deployment's inventory and will go stale as scripts change.
