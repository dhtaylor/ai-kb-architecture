---
name: knowledge-architecture-index
description: Router for how this governed knowledge base and its thin-agent layer are designed to work — conventions, scoping, lifecycle, guardrails and agent contracts.
memory_type: reference
domain: knowledge-architecture
scope: general
freshness_horizon: 90d
verifier_budget: 5
sweep_interval: 30d
last_swept: 2026-10-02
metadata:
  type: index
  node_type: router
  created: 2026-09-19
tags: [knowledge-architecture]
keywords: [knowledge base, architecture, thin agent, retrieval contract, scoping, provenance, currency, lifecycle, guardrails, librarian]
---
# Knowledge architecture

How this knowledge base and the agent layer over it are designed to work: the separation of
knowledge from behavior, the scope tiers and their homes, provenance and currency, the retrieval
contract agents are held to, the lifecycle roles that keep facts current, and the guardrails that
keep the whole thing honest.

Scope tiers, provenance, currency, contradictions, the retrieval contract and the frontmatter
schema are the operative contract and are not duplicated here. The contract lives in the engine:
`ai-kb:CONVENTIONS.md`. Repo-qualified, not a relative path — this domain is portable and must not
assume the engine sits at any particular place beside it.

- [design-thesis](design-thesis.md) — why knowledge and behavior are separated, the
  knowledge-as-behavior anti-pattern, and the target layering (three repository kinds)
- [agent-lifecycle-roles](agent-lifecycle-roles.md) — the Curator/Research-librarian/Fact-checker
  roles with the Verifier and Watcher as built, governance for KB mutation (built, dormant and
  not-built controls), how the fact-checkers are tested, and why no orchestrator is built and how
  retrieval cost is bounded
- [secret-governance](secret-governance.md) — why rotating a leaked secret isn't removal, why a
  pre-commit scan isn't the enforcement boundary, what the three-pass secret and identifier scan
  covers and cannot see, why capture refuses a note holding a credential, and the blind trap test
- [guardrail-verification](guardrail-verification.md) — why a guardrail correct on the authoring
  machine can still fail everywhere else, or catch nothing at all
- [sources](sources/INDEX.md) — citation stubs for the artifacts these facts came from

The library also holds `knowledge-architecture-golden.md` — the retrieval oracle — and `documents/`,
the archived evidence these facts were folded from. Neither is routed: they are apparatus and
provenance, not knowledge, and an agent that can descend to the oracle can read the answers.

- [episodic](episodic/INDEX.md) — session notes, newest first: the raw record facts are distilled from
- [knowledge-architecture-open-questions](knowledge-architecture-open-questions.md) — gaps the sources left open, recorded rather than filled
- [needs-attention](needs-attention.md) — open findings from automated checks and sweeps; detection only, nothing here was fixed automatically
