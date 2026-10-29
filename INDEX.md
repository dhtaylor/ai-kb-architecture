---
name: knowledge-architecture-index
description: Router for how this governed knowledge base and its thin-agent layer are designed to work — conventions, scoping, lifecycle, guardrails and agent contracts.
memory_type: reference
domain: knowledge-architecture
scope: general
freshness_horizon: 90d
agent_ttl: 30d
verifier_budget: 15
sweep_interval: 30d
kb_contract: "2.3"
last_swept: 2026-10-28
metadata:
  type: index
  node_type: router
  created: 2026-09-19
tags: [knowledge-architecture]
keywords: [knowledge base, architecture, thin agent, retrieval contract, scoping, provenance, currency, lifecycle, guardrails, librarian, retrieval eval, golden set, oracle, release, contract version, kb_contract, unattended, review packet, schedule, section read, scope line, redact]
---
# Knowledge architecture

Not covered here: Claude Code's own runtime mechanisms, such as its settings, its hook system, plugin loading and tool semantics (the engine's own hooks are covered here) — held by [[claude-code-runtime:behaviour-loading]]; the contract text itself — in ai-kb CONVENTIONS.md, not a library

- [design-thesis](design-thesis.md) — why knowledge and behavior are separated, the knowledge-as-behavior anti-pattern, the three repository kinds, library registration and its states
- [releases-and-contract-versioning](releases-and-contract-versioning.md) — engine semantic versioning and the two-day freeze, the library contract's own MAJOR.MINOR version, contract versions 2.0 to 2.3, pinning as the library owner's business
- [agent-lifecycle-roles](agent-lifecycle-roles.md) — the Curator, Research librarian and Fact-checker roles, the Verifier, Watcher and URL liveness check as built, mutation governance, agent TTL, the session-start nudge, why no orchestrator is built
- [unattended-preparation](unattended-preparation.md) — why preparing may run unattended while deciding stays attended: review packets, the weekly report, the opt-in schedule, schedulable commands, the verified backup
- [retrieval-agent-behavior](retrieval-agent-behavior.md) — how the retrieval agent decides where to look: what counts as the knowledge base, section reads, scoped search, the scope line, the subagent library list
- [secret-governance](secret-governance.md) — why rotation is not removal, why a pre-commit scan is not the boundary, the three-pass scan and what it cannot see, capture refusal, the redaction filter
- [guardrail-verification](guardrail-verification.md) — why a guardrail correct on the authoring machine fails elsewhere: defects found only by running or by a fresh reader, recurring defects, tests that pass for the wrong reason
- [retrieval-eval-integrity](retrieval-eval-integrity.md) — why a retrieval eval's score can mean nothing about the knowledge: a reachable oracle, grader defects, a moved protocol, one trial per case, cost and sample measurement
- [sources](sources/INDEX.md) — citation stubs for the artifacts these facts came from
- [episodic](episodic/INDEX.md) — session notes, newest first: the raw record facts are distilled from
- [knowledge-architecture-open-questions](knowledge-architecture-open-questions.md) — gaps the sources left open, recorded rather than filled
- [needs-attention](needs-attention.md) — the queue of findings from automated checks and sweeps, resolved items kept as history; detection only
