---
name: needs-attention
description: Open findings from automated checks (kb-watch, kb-verify) and hygiene sweeps (kb-audit), by severity.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: index
  node_type: memory
  created: 2026-10-02
tags: [meta, hygiene, queue, watcher]
keywords: [needs attention, queue, findings, drift, watcher, unreachable]
---
# Needs attention

Findings from automated drift checks and hygiene sweeps. Detection only — nothing here was fixed automatically. See CONVENTIONS.md §3 and plugins/knowledge-tools/skills/kb-audit/SKILL.md for the queue's conventions.

kb-audit sweep 2026-10-28, run as the acceptance gate for the fold of the engine's changes since the
sweep of 2026-10-16: no new item raised, and no item is open. The mechanical checks (`check-kb`,
`check-scope`, `check-golden`, `check-xlinks --installed`) passed on the folded tree. The sweep found no
section with a `Source:` and no stamp, and no stale section: the oldest stamp is 2026-10-12, and an agent
stamp falls due after 30 days, on 2026-11-11. It found no orphan, no file listed by two routers, no routed
golden set, no empty routed leaf, no `CONFLICTED` or `UNCORROBORATED` section (so the golden set needs no
unresolved or uncorroborated case), and 8 topic files, below the flat threshold. The golden set has
well-formed records with at least two negative cases and a cross-root case, and each excerpt was found in
its expected file. Two episodic notes await distillation by the owner's decision; they are not a finding.
Not covered by any check: whether each folded fact matches its source (a human diff), and the liveness of
external `Source:` URLs. The items below are the earlier sweeps' records, all resolved, kept as the queue's
history. Re-stamping without re-verifying is not a fix.

```yaml
- id: 0ff2e48a
  category: stale fact
  status: resolved
  file: agent-lifecycle-roles.md
  section: "Mutation governance"
  severity: 5
  owner: "@dhtaylor"
  stamp: "2026-09-19 · by: agent · method: doc-review"
  age: 13d
  escalate: false
  date: 2026-10-02
  resolved: "2026-10-02 — fold-layering-roles (fold a of three): section re-read and re-stamped against the revised combination plan, ADRs and script headers, method doc-review"

- id: 1e1a9e28
  category: stale fact
  status: resolved
  file: design-thesis.md
  section: "Target layering"
  severity: 5
  owner: "@dhtaylor"
  stamp: "2026-09-19 · by: agent · method: doc-review"
  age: 13d
  escalate: false
  date: 2026-10-02
  resolved: "2026-10-02 — fold-layering-roles (fold a of three): section re-read and re-stamped against the revised combination plan, ADRs and script headers, method doc-review"

- id: 3f2fc44c
  category: stale fact
  status: resolved
  file: agent-lifecycle-roles.md
  section: "Watcher safety design"
  severity: 5
  owner: "@dhtaylor"
  stamp: "2026-09-19 · by: agent · method: doc-review"
  age: 13d
  escalate: false
  date: 2026-10-02
  resolved: "2026-10-02 — fold-layering-roles (fold a of three): section re-read and re-stamped against the revised combination plan, ADRs and script headers, method doc-review"

- id: 437a83a9
  category: stale fact
  status: resolved
  file: agent-lifecycle-roles.md
  section: "The three roles"
  severity: 5
  owner: "@dhtaylor"
  stamp: "2026-09-19 · by: agent · method: doc-review"
  age: 13d
  escalate: false
  date: 2026-10-02
  resolved: "2026-10-02 — fold-layering-roles (fold a of three): section re-read and re-stamped against the revised combination plan, ADRs and script headers, method doc-review"

- id: 468657c9
  category: stale fact
  status: resolved
  file: agent-lifecycle-roles.md
  section: "Cautions: human ownership and data handling"
  severity: 5
  owner: "@dhtaylor"
  stamp: "2026-09-19 · by: agent · method: doc-review"
  age: 13d
  escalate: false
  date: 2026-10-02
  resolved: "2026-10-02 — fold-layering-roles (fold a of three): section re-read and re-stamped against the revised combination plan, ADRs and script headers, method doc-review"

- id: 7cda739f
  category: stale fact
  status: resolved
  file: design-thesis.md
  section: "The separation principle"
  severity: 5
  owner: "@dhtaylor"
  stamp: "2026-09-19 · by: agent · method: doc-review"
  age: 13d
  escalate: false
  date: 2026-10-02
  resolved: "2026-10-02 — fold-lessons-c1 (fold c1, general lessons): section re-read against the revised combination plan's section 0 (textually identical to the original), supersede note added for the freshness-loop bullet, re-stamped, method doc-review"

- id: a06fcd59
  category: stale fact
  status: resolved
  file: agent-lifecycle-roles.md
  section: "Orchestrator cost bounds"
  severity: 5
  owner: "@dhtaylor"
  stamp: "2026-09-19 · by: agent · method: doc-review"
  age: 13d
  escalate: false
  date: 2026-10-02
  resolved: "2026-10-02 — fold-layering-roles (fold a of three): section re-read and re-stamped against the revised combination plan, ADRs and script headers, method doc-review"

- id: b615b3c2
  category: stale fact
  status: resolved
  file: agent-lifecycle-roles.md
  section: "Testing the fact-checkers"
  severity: 5
  owner: "@dhtaylor"
  stamp: "2026-09-19 · by: agent · method: doc-review"
  age: 13d
  escalate: false
  date: 2026-10-02
  resolved: "2026-10-02 — fold-layering-roles (fold a of three): section re-read and re-stamped against the revised combination plan, ADRs and script headers, method doc-review"

- id: b9e0e341
  category: stale fact
  status: resolved
  file: design-thesis.md
  section: "The anti-pattern this rejects: knowledge-as-behavior"
  severity: 5
  owner: "@dhtaylor"
  stamp: "2026-09-19 · by: agent · method: doc-review"
  age: 13d
  escalate: false
  date: 2026-10-02
  resolved: "2026-10-02 — fold-lessons-c1 (fold c1, general lessons): section re-read against the revised combination plan's section 0 (textually identical to the original), re-stamped, method doc-review"

- id: 0260bac8
  category: stale fact
  status: resolved
  file: guardrail-verification.md
  section: "Environment differences hide a defect from the authoring machine; guard by structure, not by filename"
  severity: 5
  owner: "@dhtaylor"
  stamp: "2026-09-22 · by: agent · method: manual"
  age: 10d
  escalate: false
  date: 2026-10-02
  resolved: "2026-10-02 — fold-lessons-c1 (fold c1, general lessons): section re-verified: its Recheck assertion was run from the library directory and exited 0, the cited sources were re-read, re-stamped, method manual"

- id: 2b7930a4
  category: stale-recheck
  status: resolved
  file: agent-lifecycle-roles.md
  section: "Orchestrator cost bounds"
  severity: 5
  stamp: "2026-10-02 · by: agent · method: doc-review"
  age: 10d
  escalate: false
  date: 2026-10-12
  finding: "Not restamped. The section says each agent read all six INDEX.md files once (as ADR-0011 does). The sample's own evidence README at engine c668ccd (documents/eval-2026-09-28-routing-sample/README.md) says two batches read all six routers up front and one read only the three it needed. The cited decision and the evidence behind it disagree; every other claim in the section matched its sources."
  resolved: "2026-10-12 — owner-approved correction: the section now says two of the three agents read all six INDEX.md files and the third read only the three it needed, matching engine ADR-0011 as amended 2026-10-12 (engine 7541729) and the sample README; the Source line cites the amended ADR, its source stub is re-pinned to 7541729, and the section is re-stamped, method doc-review"

- id: 8e1bd4ff
  category: stale-recheck
  status: resolved
  file: secret-governance.md
  section: "What check-secrets covers: three passes"
  severity: 5
  stamp: "2026-10-02 · by: agent · method: manual"
  age: 10d
  escalate: false
  date: 2026-10-12
  finding: "Not restamped. The paragraph records a one-off synthetic run (labelled password, email, home path and domain account blocked under --file; lint-allow passed; untracked file caught by --worktree). No cited source records that run, so it cannot be confirmed from sources. Re-run on 2026-10-12 against engine c668ccd, the behaviour it describes was observed again; the owner decides whether to restamp with method manual."
  resolved: "2026-10-12 — owner-approved: the one-off synthetic-run paragraph was re-run against engine 7541729 in temp dirs (labelled password, email, home path and domain account each exit 1 under --file; lint-allow exit 0; untracked file caught by --worktree), the commands and observed values are in the PR body, and it is re-stamped method manual; a Recheck assertion now defends it"

- id: 5bbbb01c
  category: stale-recheck
  status: resolved
  file: secret-governance.md
  section: "The capture boundary: refuse before anything is written"
  severity: 5
  stamp: "2026-10-02 · by: agent · method: manual"
  age: 10d
  escalate: false
  date: 2026-10-12
  finding: "Not restamped. The paragraph records a one-off synthetic run of kb-capture (refusal leaves the tree byte-identical, --from file untouched, piped draft saved mode 0600 outside the tree, clean note filed). No cited source records that run, so it cannot be confirmed from sources. Re-run on 2026-10-12 against engine c668ccd, the refusal, exit 1, untouched tree, 0600 draft and clean filing were observed again; the owner decides whether to restamp with method manual."
  resolved: "2026-10-12 — owner-approved: the one-off synthetic-run paragraph was re-run against engine 7541729 in temp dirs (refusal exit 1, tree byte-identical, --from file untouched, piped draft saved mode -rw------- outside the tree, clean note filed), the commands and observed values are in the PR body, and it is re-stamped method manual; a Recheck assertion now defends it"

```
