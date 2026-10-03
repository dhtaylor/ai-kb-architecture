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

kb-audit sweep 2026-10-02: 15 stale facts found, 10 raised (cap 10 per owner), 5 held back — the
four `guardrail-verification.md` sections other than "Environment differences…" and both
`secret-governance.md` sections, all stamped 2026-09-22 (10 days). The `secret-governance.md` fold
has since re-verified both of its two held-back sections (2026-10-02: sources re-read, Recheck
lines run), so only the four `guardrail-verification.md` sections remain held back. Every section in the library is
`by: agent`; the domain horizon is 90d, so an agent stamp is stale after 9 days. No item has passed
the 14-day escalation mark. Re-stamping without re-verifying is not a fix.

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
  status: open
  file: design-thesis.md
  section: "The separation principle"
  severity: 5
  owner: "@dhtaylor"
  stamp: "2026-09-19 · by: agent · method: doc-review"
  age: 13d
  escalate: false
  date: 2026-10-02

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
  status: open
  file: design-thesis.md
  section: "The anti-pattern this rejects: knowledge-as-behavior"
  severity: 5
  owner: "@dhtaylor"
  stamp: "2026-09-19 · by: agent · method: doc-review"
  age: 13d
  escalate: false
  date: 2026-10-02

- id: 0260bac8
  category: stale fact
  status: open
  file: guardrail-verification.md
  section: "Environment differences hide a defect from the authoring machine; guard by structure, not by filename"
  severity: 5
  owner: "@dhtaylor"
  stamp: "2026-09-22 · by: agent · method: manual"
  age: 10d
  escalate: false
  date: 2026-10-02

```
