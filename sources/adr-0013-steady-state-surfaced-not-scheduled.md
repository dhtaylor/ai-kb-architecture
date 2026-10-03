---
name: adr-0013-steady-state-surfaced-not-scheduled
description: Source stub — engine ADR-0013, steady state is surfaced when due, not run unattended.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-10-02
tags: [knowledge-architecture, source]
keywords: [cadence, nudge, kb-due, queue, Watcher snapshots, ADR]
---
# Source: engine ADR-0013, steady state is surfaced when due, not run unattended

- **Title:** ADR-0013: Steady state is surfaced when due, not run unattended
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** 2026-10-01
- **Date ingested:** 2026-10-02
- **Artifact location:** `ai-kb:documents/decisions/0013-steady-state-surfaced-not-scheduled.md`, read at engine commit `35f0e95` on 2026-10-02. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.

The engine's decision record for how the Verifier, Watcher and sweeps reach a human. Folded from it: cadence is a nudge at session start rather than an unattended schedule, both fact-checkers feed one routed queue, and the limit that a watched source is only as fresh as its last saved snapshot. Not folded: deployment specifics (which source is watched first, what is gitignored and why, the contributor guide's length).
