---
name: design-thesis
description: The core design thesis of a governed-KB-plus-thin-agent architecture — why knowledge and behavior are separated, the anti-pattern this rejects, and the target layering (three repository kinds: engine, domain library, project knowledge).
memory_type: semantic
domain: knowledge-architecture
scope: general
verified: 2026-09-19
metadata:
  type: fact
  node_type: memory
  created: 2026-09-19
tags: [knowledge-architecture, design, thin-agent, governed-kb]
keywords: [design thesis, knowledge as behavior, knowledge as document, anti-pattern, freshness engine, target architecture, thin agent layer, guardrails]
---
# Design thesis: separate knowledge from behavior

## The separation principle

The core design rule for combining a governed knowledge base with an agent layer:
**separate knowledge from behavior, and let the behavior layer keep the knowledge layer honest.**

- **Knowledge** (facts: schemas, system behavior, documented bugs, topology) lives *only* in the
  governed KB — one canonical home, provenance, dedup, contradiction handling.
- **Behavior** (agents + commands) carries *only* routing, procedure, tool scoping, and
  confirmation gates. It holds **no embedded facts** — it retrieves them from the KB at runtime
  and acts.
- **The loop that makes it more than either half:** because the behavior layer can *act* (query
  live systems), it becomes the KB's freshness engine — agents re-verify facts against the live
  system and re-stamp them. The KB gains currency; the behavior layer gains a single source of
  truth.

Source: [[knowledge-agent-architecture-combination-plan]] (§0)
Verified: 2026-09-19 · by: agent · method: doc-review

## The anti-pattern this rejects: knowledge-as-behavior

Two opposite ways to hold operational knowledge, compared property by property:

| Property | Knowledge-as-behavior (facts frozen in agent prompts) | Knowledge-as-document (a governed KB) |
|---|---|---|
| Single canonical fact | N copies, drift | one home |
| Provenance / auditability | none | per-section backlinks |
| Currency of facts | unmanaged | cited but unverified |
| Executable / operational | it acts | inert |
| Context economy at query time | whole prompt loads | progressive disclosure |
| Maintenance cost | scales with #agents | high ceremony |
| Team shareability | obvious, in-repo | can become bespoke/solo |
| Safety (secrets/leaks) | inlined | inert, nothing to leak |

The governed KB is the superior *knowledge* design; a behavior fleet is the superior *execution*
design. The optimum is the KB as substrate with a thin behavior layer on top of it.
**Knowledge-as-behavior is retained only as the anti-pattern to avoid** — the failure it embodies
(duplicated, unsourced, drifting facts) is exactly what the governed substrate prevents.

Source: [[knowledge-agent-architecture-combination-plan]] (§0)
Verified: 2026-09-19 · by: agent · method: doc-review

## Target layering

The architecture is three kinds of repository, not one tree holding both the governed substrate and
the behaviour layer:

- **Engine** — behaviour only, no domains, no facts. It holds the contract every library conforms
  to, the checking scripts, the plugins (skills, agents, commands — registered by consumers, never
  inherited by directory accident) and the decision records. Its `kb/` directory is created by the
  engine and never tracked by it.
- **Domain library** — content only, one repository per domain, portable. `INDEX.md` is the
  repository root and the domain router, with no tier chain above it. It holds gestalt-sized topic
  files with per-section provenance and currency, its own `sources/`, the `documents/` it was folded
  from, and its own golden set.
- **Project knowledge** — the repo tier, inside each project repository, travelling with the clone.

Two cross-cutting layers sit beside them. **Secrets** live in a vault or the environment only,
referenced by name, never inlined. **Guardrails** are a secret scan (committer-side and server-side)
plus an embedded-fact lint over the behaviour layer, and least-privilege enforcement for production
operations.

The rule that makes it hold: the engine holds no facts, a library holds no behaviour, and a library
is self-contained enough to be cloned onto a machine with neither an engine nor a sibling library
beside it. Anything reaching across a repository boundary uses a repo-qualified reference
(`ai-kb:CONVENTIONS.md`), never a relative path; the contract states the citation rule at
`ai-kb:CONVENTIONS.md` section 6.

Why the engine does not hold the facts: an engine that held both would assert the separation
principle in its own contract and violate it in its own layout, in the one place where the
separation most needs to be visible, because everything downstream follows its example. There was
also a practical failure: with one shared tier there was one shared `sources/`, one shared golden
set and one router chain, so a domain could not be moved, shared or cloned without dragging the rest
of the tier with it. There is no registry; libraries are discovered by being present under the
engine's `kb/`, because a registry would have to live in the engine, which does not track `kb/`.

The cost, stated by the decision that made the split: a library has no guardrails of its own. Its
commit hook borrows the engine's checkers and skips when it cannot find them, so a library cloned
alone is unguarded until an engine is in reach. That is the price of not making a portable library
depend on an engine.

Superseded 2026-10-02: the earlier text of this section described one `knowledge/` tree as the
governed substrate, a thin behaviour layer of domain-worker agents with least-privilege tools routed
by a thin orchestrator, and a secret scan over both trees. The plan rewrote its target architecture
on 2026-09-19 to the three repository kinds above, and no orchestrator is built (see
[[agent-lifecycle-roles]], "Orchestrator cost bounds"). Least-privilege enforcement remains in the
guardrails list; its as-built strength is recorded under "Mutation governance" in the same file.

**How this holds in practice depends on the runtime.** The claim that behaviour "retrieves at
runtime" is about where facts live, not about when the behaviour itself is loaded — and those differ:
in Claude Code, plugin behaviour is loaded once at session start while facts are read live per query
(`[[claude-code-runtime:behaviour-loading]]`). The separation holds; the two halves simply refresh on
different clocks.

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§1) · [[knowledge-agent-architecture-combination-plan]] (§1, superseded) · [[adr-0007-engine-content-separation]]
Verified: 2026-10-02 · by: agent · method: doc-review
