---
name: adr-0009-recheck-assertions
description: Source stub — engine ADR-0009, facts defend themselves with an assertion and the verifier reports and does not repair.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-10-02
tags: [knowledge-architecture, source]
keywords: [verifier, Recheck, assertion, negative control, kb-verify, ADR]
---
# Source: engine ADR-0009, facts defend themselves with an assertion and the verifier reports and does not repair

- **Title:** ADR-0009: Facts defend themselves with an assertion; the verifier reports and does not repair
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** 2026-09-22
- **Date ingested:** 2026-10-02
- **Artifact location:** `ai-kb:documents/decisions/0009-recheck-assertions.md`, read at engine commit `35f0e95` on 2026-10-02. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.

The engine's decision record for the Verifier as built. Folded from it: the three rules (pass restamps only with --apply, a failure is never applied, commands print and run only with --yes), that most facts carry no assertion, and that an assertion must be able to fail. Its test-evidence table is a dated run record and was not folded.
