---
name: knowledge-architecture-index
description: Router for how this governed knowledge base and its thin-agent layer are designed to work — conventions, scoping, lifecycle, guardrails and agent contracts.
memory_type: reference
domain: knowledge-architecture
scope: general
freshness_horizon: 90d
agent_ttl: 30d
verifier_budget: 5
sweep_interval: 30d
kb_contract: "2.3"
last_swept: 2026-10-16
metadata:
  type: index
  node_type: router
  created: 2026-09-19
tags: [knowledge-architecture]
keywords: [knowledge base, architecture, thin agent, retrieval contract, scoping, provenance, currency, lifecycle, guardrails, librarian, retrieval eval, golden set, oracle, release, contract version, kb_contract, unattended, review packet, schedule, section read, scope line, redact]
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

Not covered here: Claude Code runtime behaviour (settings, hooks, plugin loading, tool semantics) — held by [[claude-code-runtime:behaviour-loading]]; the contract text itself — in ai-kb CONVENTIONS.md, not a library

- [design-thesis](design-thesis.md) — why knowledge and behavior are separated, the
  knowledge-as-behavior anti-pattern, and the target layering (three repository kinds), including
  why a library is installed by registration and carries no hook of its own, and the states a
  registered library can be in
- [releases-and-contract-versioning](releases-and-contract-versioning.md) — how the engine is versioned
  and released (semantic versioning, the two-day freeze), why the library contract has its own
  MAJOR.MINOR version that a library declares, how rules apply by it, what a library behind, ahead or on
  an unsupported major is told, what contract versions 2.0 to 2.3 each add, and why pinning is the
  library owner's business
- [agent-lifecycle-roles](agent-lifecycle-roles.md) — the Curator/Research-librarian/Fact-checker
  roles with the Verifier (and its unattended form) and Watcher as built, governance for KB mutation
  (built, dormant and not-built controls), the source-URL liveness check and its request sequence, why
  agent stamps may declare their own TTL, how the session-start nudge is built and reaches the person,
  how the fact-checkers are tested, the steady-state defaults (silent when idle, cheap, routed), and why
  no orchestrator is built and how retrieval cost is bounded
- [unattended-preparation](unattended-preparation.md) — why preparing knowledge work may run unattended
  while deciding stays attended: review packets, why a headless run cannot write, the weekly report, the
  opt-in schedule, what makes a command safe to schedule, the verified backup, and what is never automated
- [retrieval-agent-behavior](retrieval-agent-behavior.md) — how the retrieval agent decides where to look:
  what counts as the knowledge base and why the engine repository does not, section-targeted reads, why a
  search is scoped to the routed library, how the scope line and the registration overlap warning separate
  neighboring libraries, and how a subagent is given the library list
- [secret-governance](secret-governance.md) — why rotating a leaked secret isn't removal, why a
  pre-commit scan isn't the enforcement boundary, what the three-pass secret and identifier scan
  covers and cannot see, why raw artifacts archived in a library skip one rule and UTF-16 is
  blocked, why capture refuses a note holding a credential, how the redaction filter keeps a secret out of an
  unattended model's output, and the blind trap test
- [guardrail-verification](guardrail-verification.md) — why a guardrail correct on the authoring
  machine can still fail everywhere else, or catch nothing at all: defects found only by running or by
  a fresh reader, a bug that recurs, a scanner that exits 0 after printing findings
- [retrieval-eval-integrity](retrieval-eval-integrity.md) — why a retrieval eval's score can mean
  nothing about the knowledge: a reachable oracle, a grader testing markup or strings, a grader
  demanding a quote nobody asked for, an oracle that follows the agent, a wrong or partial corpus, a
  moved protocol, a clean control nobody checked
- [sources](sources/INDEX.md) — citation stubs for the artifacts these facts came from

The library also holds `knowledge-architecture-golden.md` — the retrieval oracle — and `documents/`,
the archived evidence these facts were folded from. Neither is routed: they are apparatus and
provenance, not knowledge, and an agent that can descend to the oracle can read the answers.

- [episodic](episodic/INDEX.md) — session notes, newest first: the raw record facts are distilled from
- [knowledge-architecture-open-questions](knowledge-architecture-open-questions.md) — gaps the sources left open, recorded rather than filled
- [needs-attention](needs-attention.md) — open findings from automated checks and sweeps; detection only, nothing here was fixed automatically
