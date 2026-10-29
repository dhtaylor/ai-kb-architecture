---
name: design-thesis
description: The core design thesis of a governed-KB-plus-thin-agent architecture — why knowledge and behavior are separated, the anti-pattern this rejects, and the target layering (three repository kinds: engine, domain library, project knowledge), including how a library is installed (registered per machine) and why it carries no hook.
memory_type: semantic
domain: knowledge-architecture
scope: general
verified: 2026-10-12
metadata:
  type: fact
  node_type: memory
  created: 2026-09-19
tags: [knowledge-architecture, design, thin-agent, governed-kb]
keywords: [design thesis, knowledge as behavior, knowledge as document, anti-pattern, freshness engine, target architecture, thin agent layer, guardrails, registry, library registration, library hook, core.hooksPath, unsupported-contract, kb_contract, contract version]
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

As built, the third bullet's loop is narrower than the plan imagined: it had agents querying live
systems to re-verify facts. The environment had none, and the Verifier was built as a script that re-runs an assertion the fact
carries beside it (`Recheck:`), with most facts carrying none; see [[agent-lifecycle-roles]], "The
Verifier as built". The separation principle itself is unchanged: section 0 reads identically in the
revised plan and the original.

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§0, §9.13) · [[knowledge-agent-architecture-combination-plan]] (§0, identical text)
Verified: 2026-10-12 · by: agent · method: doc-review

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

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§0) · [[knowledge-agent-architecture-combination-plan]] (§0, identical text)
Verified: 2026-10-12 · by: agent · method: doc-review

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
of the tier with it.

**A library is installed on a machine by being registered there.** The decision that made it so:
"A library is installed on a machine when it is registered there, and only then. `<engine>/kb/`
stops being special: it is the default folder new libraries are cloned into, and nothing scans it."
The registry is a per-machine file outside every repository, `~/.config/ai-kb/libraries`. "It
describes the machine it is on, so it cannot lie about another one." A library's name is the
`domain:` value in its root `INDEX.md`, so the folder holding it can be called anything. The reason
discovery by presence was replaced: "A library that disappears fails silently. If a library folder
is moved or deleted, it is simply no longer enumerated. Retrieval then answers `fact not found` for
everything it held, which looks exactly like a correct refusal." Each registered library is
reported in one state: `ok`, `missing`, `not-a-library`, `name-mismatch` or `unsupported-contract`.
"Only `ok` libraries are served. Every other state is reported at session start, by name and path,
and never dropped silently." The fifth state is a library whose declared contract version names a
major the engine does not support, or cannot be read: its markers may mean something the engine
cannot read, and serving it could present a disputed fact as settled (see
[[releases-and-contract-versioning]]).

The cost, stated by the decision that made the split: a library has no guardrails of its own, so a
library cloned alone is unguarded until an engine is in reach. That is the price of not making a
portable library depend on an engine. What ties a library to the engine's rules is the contract
version it declares, not a pin on the engine's code, so the engine can tighten a rule without a
library built correctly to the earlier rules becoming unusable ([[releases-and-contract-versioning]]).
A library now carries no hook logic either. "A library is
shareable: one repository, cloned by any number of people who do not own it", so registering it,
moving it or upgrading the engine can never require a commit to it. The pre-commit hook lives in the
engine, and registering a clone sets its `core.hooksPath` to the engine's hook folder: "This is local
git configuration. Git never commits or clones it, so every clone carries its own".

The plan's earlier target was one `knowledge/` tree as the governed substrate, a thin behavior layer
of domain-worker agents with least-privilege tools routed by a thin orchestrator, and a secret scan over
both trees. It was rewritten to the three repository kinds above, and no orchestrator is built (see
[[agent-lifecycle-roles]], "Orchestrator cost bounds"). Least-privilege enforcement remains in the
guardrails list; its as-built strength is recorded under "Mutation governance" in the same file.

**How this holds in practice depends on the runtime.** The claim that behaviour "retrieves at
runtime" is about where facts live, not about when the behaviour itself is loaded — and those differ:
in Claude Code, plugin behaviour is loaded once at session start while facts are read live per query
(`[[claude-code-runtime:behaviour-loading]]`). The separation holds; the two halves simply refresh on
different clocks.

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§1) · [[knowledge-agent-architecture-combination-plan]] (§1, superseded) · [[adr-0007-engine-content-separation]] (amended) · [[adr-0014-library-registry]] · [[script-kb-libraries]] · [[adr-0016-versioned-library-contract]]
Verified: 2026-10-28 · by: agent · method: doc-review
