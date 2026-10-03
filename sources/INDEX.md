---
name: sources-index
description: Citation registry for this domain — stubs for the artifacts its facts were folded from.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: index
  node_type: router
  created: 2026-09-19
tags: [knowledge-architecture, sources, index]
keywords: [sources, citations, stubs, provenance]
---
# Sources

- [knowledge-agent-architecture-combination-plan](knowledge-agent-architecture-combination-plan.md) — the architecture proposal this domain was folded from
- [knowledge-agent-architecture-combination-plan-revised](knowledge-agent-architecture-combination-plan-revised.md) — the plan as revised from build findings; supersedes the entry above
- [adr-0002-governance-without-enforcement](adr-0002-governance-without-enforcement.md) — engine decision: governance written in full, enforced only where enforceable
- [adr-0007-engine-content-separation](adr-0007-engine-content-separation.md) — engine decision: the engine holds no knowledge; content lives in per-domain libraries
- [adr-0009-recheck-assertions](adr-0009-recheck-assertions.md) — engine decision: facts carry an assertion; the verifier reports, never repairs
- [adr-0011-no-orchestrator-until-routing-fails](adr-0011-no-orchestrator-until-routing-fails.md) — engine decision: no orchestrator built; revisit triggers
- [adr-0012-phase-4-safety-scoped-to-one-maintainer](adr-0012-phase-4-safety-scoped-to-one-maintainer.md) — engine decision: safety controls as built, declared versus enforced, dormant
- [adr-0013-steady-state-surfaced-not-scheduled](adr-0013-steady-state-surfaced-not-scheduled.md) — engine decision: cadence is surfaced when due, not run unattended
- [state-change-gates](state-change-gates.md) — engine document: every state-changing entry point against the three automation bands
- [script-kb-verify](script-kb-verify.md) — header of the Verifier script
- [script-kb-watch](script-kb-watch.md) — header of the Watcher script
- [script-kb-due](script-kb-due.md) — header of the cadence-nudge script
- [script-check-blast](script-check-blast.md) — header of the high-blast-radius gate script
- [script-kb-owner](script-kb-owner.md) — header of the CODEOWNERS resolver script
