---
name: knowledge-architecture-golden
description: Golden retrieval set for the knowledge-architecture domain — routing and answer-grounding cases seeded by the first fold (design-thesis, agent-lifecycle-roles, secret-governance).
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: reference
  node_type: golden-set
  created: 2026-09-19
tags: [knowledge-architecture, golden-set, eval]
keywords: [golden set, retrieval eval, negative case, positive case]
---
# Knowledge-architecture golden set

Excerpts are **markup-free prose**. An excerpt carrying markdown emphasis can exist in the file and
never in an answer, so it tests formatting rather than grounding — found by the first eval run, where
a correct answer failed on a pair of asterisks.

This file is **test apparatus, not knowledge**. It is deliberately NOT routed from the domain
`INDEX.md`: an agent that can descend to the oracle can read the answers, and its refusals then prove
nothing. Retrieval agents must never read it.

Seeded by the first fold of `knowledge-agent-architecture-combination-plan.md`. Cross-root case:
**not applicable** — this domain now holds one cross-library reference, into
claude-code-runtime, so a cross-root case is included. Unresolved case: **not applicable** — this fold produced no contradiction (see the fold
report); nothing in the domain today carries `status: CONFLICTED`, so a genuine UNRESOLVED case
cannot yet be written without fabricating a conflict. Add one the day a real contradiction lands.

```yaml
- case: positive
  question: What is the core design rule for combining a governed KB with an agent layer?
  expected_file: design-thesis.md
  expected_excerpt: "separate knowledge from behavior, and let the behavior layer keep the knowledge layer honest"

- case: positive
  question: What are the three librarian roles that keep this kind of KB current, and which one splits into two?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "Curator (cataloguer)"

- case: positive
  question: How many workers can the orchestrator fan out to for a single interactive query?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "capped at 2 workers"

- case: positive
  question: Does rotating a leaked secret remove it from git history?
  expected_file: secret-governance.md
  expected_excerpt: "does not erase it from git"

- case: negative
  question: What is the production database's deploy target for this workspace?
  expected_file: ""
  expected_excerpt: "fact not found — check KB"

- case: negative
  question: What did Dandy's personal debugging session on Tuesday conclude?
  expected_file: ""
  expected_excerpt: "fact not found — check KB"

- case: cross-root
  question: Does the behaviour layer being "thin" mean it is reloaded whenever knowledge changes?
  expected_file: design-thesis.md
  expected_excerpt: "loaded once at session start while facts are read live per query"
```
