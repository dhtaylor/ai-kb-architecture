---
name: adr-0012-phase-4-safety-scoped-to-one-maintainer
description: Source stub — engine ADR-0012, Phase 4 safety hardening scoped to what a single maintainer can enforce.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-10-02
tags: [knowledge-architecture, source]
keywords: [least privilege, tool scope, Watcher, Verifier test, high-blast gate, dormant, ADR]
---
# Source: engine ADR-0012, Phase 4 safety hardening scoped to what a single maintainer can enforce

- **Title:** ADR-0012: Phase 4 safety hardening, scoped to what one maintainer on one machine can enforce
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** 2026-09-29
- **Date ingested:** 2026-10-02
- **Artifact location:** `ai-kb:documents/decisions/0012-phase-4-safety-scoped-to-one-maintainer.md`, read at engine commit `35f0e95` on 2026-10-02. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.

The engine's decision record for the safety controls as built. Folded from it: which controls are declared versus enforced, that the Verifier is tested as built, that the Watcher is offline and model-free, the high-blast gate's scope, and which controls are dormant. Not folded: hosting-platform label-permission and admin-bypass mechanics (product behaviour), and the runtime's own tool-scoping semantics (belongs in the claude-code-runtime library, which holds no slug for it yet).
