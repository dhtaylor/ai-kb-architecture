---
name: knowledge-architecture-golden
description: Golden retrieval set for the knowledge-architecture domain — routing and answer-grounding cases for design-thesis, agent-lifecycle-roles, secret-governance and guardrail-verification.
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

Seeded by the first fold of `knowledge-agent-architecture-combination-plan.md`; extended by the fold
of the revised plan (target layering and lifecycle roles as built). Cross-root case: included — the
domain holds one cross-library reference, from design-thesis into claude-code-runtime
(`[[claude-code-runtime:behaviour-loading]]`), and the `cross-root` record below tests it. Unresolved
case: **not applicable** — nothing in the domain today carries `status: CONFLICTED`, so a genuine
UNRESOLVED case cannot yet be written without fabricating a conflict. Add one the day a real
contradiction lands.

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
  question: Is there an orchestrator that routes questions across libraries, and if not, what does the routing?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "no orchestrator is built"
  alt_excerpt: "the system built has one retrieval agent"

- case: positive
  question: Does rotating a leaked secret remove it from git history?
  expected_file: secret-governance.md
  expected_excerpt: "does not erase it from git"

- case: negative
  question: What is the production database's deploy target for this workspace?
  expected_file: ""
  expected_excerpt: "fact not found — check KB"

- case: negative
  question: What did the maintainer's personal debugging session on Tuesday conclude?
  expected_file: ""
  expected_excerpt: "fact not found — check KB"

- case: cross-root
  question: Does the behaviour layer being "thin" mean it is reloaded whenever knowledge changes?
  expected_file: design-thesis.md
  expected_excerpt: "loaded once at session start while facts are read live per query"

- case: positive
  question: Why did check-exec-bits not catch kb-bootstrap and kb-session-start being committed non-executable?
  expected_file: guardrail-verification.md
  expected_excerpt: "rather than what it is named or where it lives, so a future script needs no naming convention to be covered"

- case: positive
  question: Why did nobody notice check-kb importing PyYAML until the regression suite ran it in CI?
  expected_file: guardrail-verification.md
  expected_excerpt: "the engine's own CI runs only exec-bit, secret and embedded-fact checks"

- case: positive
  question: Why didn't the new regression suite catch the check-xlinks episodic-exclusion regression?
  expected_file: guardrail-verification.md
  expected_excerpt: "the suite had no case citing a session note — the same shape of input the original bug lived in"

- case: positive
  question: Does a clean kb-audit sweep mean every fact's content is still accurate?
  expected_file: guardrail-verification.md
  expected_excerpt: "it does not check a fact's content against the live system that fact describes"

- case: positive
  question: How many kinds of repository does the target architecture use, and what does each hold?
  expected_file: design-thesis.md
  expected_excerpt: "three kinds of repository, not one tree"

- case: positive
  question: When the Verifier re-runs a fact's assertion and it fails, does it edit or flag the fact?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "A failure is never applied"

- case: positive
  question: Does the Watcher fetch pages itself, and does fetched page content ever reach a model?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "never fetches anything itself"
  alt_excerpt: "page content never reaches one"

- case: positive
  question: Are the knowledge-base sweeps and distillations run on a schedule unattended?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "nothing happens unless a session starts"

- case: positive
  question: Is the two-reviewer, 24-hour rule for high-blast-radius facts enforced?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "dormant until a second owner exists"

- case: positive
  question: What would make the decision not to build an orchestrator be reopened?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "Routing accuracy falls below 95%"
```
