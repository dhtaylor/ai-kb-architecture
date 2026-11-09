---
name: needs-attention
description: Findings from automated checks (kb-watch, kb-verify) and hygiene sweeps (kb-audit), by severity: the open items, and the resolved ones kept as history.
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

## Resolved history

The kb-audit sweep of 2026-10-28, run as the acceptance gate for the fold of the engine's changes since the
sweep of 2026-10-16, raised no item of its own. The mechanical checks (`check-kb`,
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
- id: 7949faa6
  category: unstamped fact
  status: resolved
  file: knowledge-architecture-open-questions.md
  section: "Is retrieval below the reopen line of the no-orchestrator decision, and what replaces one agent doing triage and specialist work?"
  source: "issue-130-triage-and-specialist-retrieval"
  severity: 4
  owner: "@dhtaylor"
  stamp: none
  age: unknown (no stamp, and no logged date for when the gap began)
  escalate: false
  date: 2026-11-04
  finding: "The section's Source: line cites [[issue-130-triage-and-specialist-retrieval]] and carries no Verified: stamp beneath it, so its currency is unknown (§7). The section records an open question rather than a fact, and the contract does not exempt open-questions sections from stamps; the owner chose on 2026-11-04 to queue these rather than exempt them. The 2026-10-28 sweep did not raise them. To close: re-read the cited source and stamp the section with the method used, or remove the question if the source has since answered it."
  resolved: "2026-11-09 — engine-sync fold B (retrieval): ADR-0018 answered the question, so it was replaced by the question still open, whether triage meets its review trigger, stamped against ADR-0018, method doc-review"

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

- id: 8846dbfc
  category: stale fact
  status: resolved
  file: sources/engine-changelog-unreleased.md
  section: "Source: ai-kb CHANGELOG, section Unreleased"
  severity: 5
  owner: "@dhtaylor"
  stamp: "2026-10-28 · by: agent · method: doc-review"
  age: 0d
  escalate: false
  date: 2026-10-28
  finding: "The stub pins the changelog's Unreleased section at engine commit d16b78c. When v0.3.0 is released that section becomes the v0.3.0 release section, and the heading the stub and the facts that cite it name no longer exists. After the release: re-pin the stub to tag v0.3.0, re-read the section it cites, re-stamp the facts that cite it, and rename the stub if its title no longer fits. Not done before the release: the section is still Unreleased, and the engine is frozen from 2026-10-29."
  resolved: "2026-10-31 — after the v0.3.0 release: the stub is renamed engine-changelog-v0.3.0 and pinned to tag v0.3.0 (commit 6a779f3), and the 10 Source lines citing it updated. git diff d16b78c v0.3.0 -- CHANGELOG.md changes only the section heading and its two-line introduction; every entry is byte-identical and no fact cites the heading or introduction, so the citing sections keep their 2026-10-28 stamps rather than being re-stamped without their other sources re-read"
```

## Open items

```yaml
- id: 0f989e88
  category: unstamped fact
  status: open
  file: knowledge-architecture-open-questions.md
  section: "Does an unneeded content read count against the routing-accuracy reopen trigger?"
  source: "eval-2026-10-12-routing-run"
  severity: 4
  owner: "@dhtaylor"
  stamp: none
  age: unknown (no stamp, and no logged date for when the gap began)
  escalate: false
  date: 2026-11-04
  finding: "The section's Source: line cites [[eval-2026-10-12-routing-run]] and carries no Verified: stamp beneath it, so its currency is unknown (§7). The section records an open question rather than a fact, and the contract does not exempt open-questions sections from stamps; the owner chose on 2026-11-04 to queue these rather than exempt them. The 2026-10-28 sweep did not raise them. To close: re-read the cited source and stamp the section with the method used, or remove the question if the source has since answered it."

- id: 1cc16ecf
  category: unstamped fact
  status: open
  file: knowledge-architecture-open-questions.md
  section: "Where does the runtime's tool-scoping behaviour belong?"
  source: "adr-0012-phase-4-safety-scoped-to-one-maintainer"
  severity: 4
  owner: "@dhtaylor"
  stamp: none
  age: unknown (no stamp, and no logged date for when the gap began)
  escalate: false
  date: 2026-11-04
  finding: "The section's Source: line cites [[adr-0012-phase-4-safety-scoped-to-one-maintainer]] and carries no Verified: stamp beneath it, so its currency is unknown (§7). The section records an open question rather than a fact, and the contract does not exempt open-questions sections from stamps; the owner chose on 2026-11-04 to queue these rather than exempt them. The 2026-10-28 sweep did not raise them. To close: re-read the cited source and stamp the section with the method used, or remove the question if the source has since answered it."

- id: 39d02c84
  category: unstamped fact
  status: open
  file: knowledge-architecture-open-questions.md
  section: "Were the machine-readable bot label, the retrieval-check diff annotation, the pull notification on rollback and the per-domain corrections-log ever built?"
  source: "knowledge-agent-architecture-combination-plan-revised"
  severity: 4
  owner: "@dhtaylor"
  stamp: none
  age: unknown (no stamp, and no logged date for when the gap began)
  escalate: false
  date: 2026-11-04
  finding: "The section's Source: line cites [[knowledge-agent-architecture-combination-plan-revised]] and carries no Verified: stamp beneath it, so its currency is unknown (§7). The section records an open question rather than a fact, and the contract does not exempt open-questions sections from stamps; the owner chose on 2026-11-04 to queue these rather than exempt them. The 2026-10-28 sweep did not raise them. To close: re-read the cited source and stamp the section with the method used, or remove the question if the source has since answered it."

- id: 54016ecc
  category: unstamped fact
  status: open
  file: knowledge-architecture-open-questions.md
  section: "Are the contract's conformance suite and the split of its engine-facing text built?"
  source: "adr-0016-versioned-library-contract"
  severity: 4
  owner: "@dhtaylor"
  stamp: none
  age: unknown (no stamp, and no logged date for when the gap began)
  escalate: false
  date: 2026-11-04
  finding: "The section's Source: line cites [[adr-0016-versioned-library-contract]] and carries no Verified: stamp beneath it, so its currency is unknown (§7). The section records an open question rather than a fact, and the contract does not exempt open-questions sections from stamps; the owner chose on 2026-11-04 to queue these rather than exempt them. The 2026-10-28 sweep did not raise them. To close: re-read the cited source and stamp the section with the method used, or remove the question if the source has since answered it."

- id: 8fe6b3bd
  category: unstamped fact
  status: open
  file: knowledge-architecture-open-questions.md
  section: "Is a human-maintained digest of external changes actually kept?"
  source: "knowledge-agent-architecture-combination-plan-revised"
  severity: 4
  owner: "@dhtaylor"
  stamp: none
  age: unknown (no stamp, and no logged date for when the gap began)
  escalate: false
  date: 2026-11-04
  finding: "The section's Source: line cites [[knowledge-agent-architecture-combination-plan-revised]] and carries no Verified: stamp beneath it, so its currency is unknown (§7). The section records an open question rather than a fact, and the contract does not exempt open-questions sections from stamps; the owner chose on 2026-11-04 to queue these rather than exempt them. The 2026-10-28 sweep did not raise them. To close: re-read the cited source and stamp the section with the method used, or remove the question if the source has since answered it."

- id: 9f80cef8
  category: unstamped fact
  status: open
  file: knowledge-architecture-open-questions.md
  section: "Does the CI cold-start advice (shallow clone at a pinned tag, sparse checkout) carry over to per-domain library repositories?"
  source: "knowledge-agent-architecture-combination-plan-revised"
  severity: 4
  owner: "@dhtaylor"
  stamp: none
  age: unknown (no stamp, and no logged date for when the gap began)
  escalate: false
  date: 2026-11-04
  finding: "The section's Source: line cites [[knowledge-agent-architecture-combination-plan-revised]] and carries no Verified: stamp beneath it, so its currency is unknown (§7). The section records an open question rather than a fact, and the contract does not exempt open-questions sections from stamps; the owner chose on 2026-11-04 to queue these rather than exempt them. The 2026-10-28 sweep did not raise them. To close: re-read the cited source and stamp the section with the method used, or remove the question if the source has since answered it."

- id: a5ae4775
  category: unstamped fact
  status: open
  file: knowledge-architecture-open-questions.md
  section: "Where does the runtime's behaviour on library discovery by a sub-agent belong?"
  source: "knowledge-agent-architecture-combination-plan-revised"
  severity: 4
  owner: "@dhtaylor"
  stamp: none
  age: unknown (no stamp, and no logged date for when the gap began)
  escalate: false
  date: 2026-11-04
  finding: "The section's Source: line cites [[knowledge-agent-architecture-combination-plan-revised]] and carries no Verified: stamp beneath it, so its currency is unknown (§7). The section records an open question rather than a fact, and the contract does not exempt open-questions sections from stamps; the owner chose on 2026-11-04 to queue these rather than exempt them. The 2026-10-28 sweep did not raise them. To close: re-read the cited source and stamp the section with the method used, or remove the question if the source has since answered it."

- id: bc131855
  category: unstamped fact
  status: open
  file: knowledge-architecture-open-questions.md
  section: "Does the redaction filter cover what the scan cannot see?"
  source: "script-check-secrets"
  severity: 4
  owner: "@dhtaylor"
  stamp: none
  age: unknown (no stamp, and no logged date for when the gap began)
  escalate: false
  date: 2026-11-04
  finding: "The section's Source: line cites [[script-check-secrets]] and carries no Verified: stamp beneath it, so its currency is unknown (§7). The section records an open question rather than a fact, and the contract does not exempt open-questions sections from stamps; the owner chose on 2026-11-04 to queue these rather than exempt them. The 2026-10-28 sweep did not raise them. To close: re-read the cited source and stamp the section with the method used, or remove the question if the source has since answered it."

- id: ccbd8499
  category: unstamped fact
  status: open
  file: knowledge-architecture-open-questions.md
  section: "Does the scope line separate neighboring libraries in practice?"
  source: "conventions-contract-versions"
  severity: 4
  owner: "@dhtaylor"
  stamp: none
  age: unknown (no stamp, and no logged date for when the gap began)
  escalate: false
  date: 2026-11-04
  finding: "The section's Source: line cites [[conventions-contract-versions]] and carries no Verified: stamp beneath it, so its currency is unknown (§7). The section records an open question rather than a fact, and the contract does not exempt open-questions sections from stamps; the owner chose on 2026-11-04 to queue these rather than exempt them. The 2026-10-28 sweep did not raise them. To close: re-read the cited source and stamp the section with the method used, or remove the question if the source has since answered it."

- id: da6e91d3
  category: unstamped fact
  status: open
  file: knowledge-architecture-open-questions.md
  section: "Does `kb-apply` change a fact file, and which list does the weekly report hold?"
  source: "adr-0017-unattended-preparation-attended-decisions"
  severity: 4
  owner: "@dhtaylor"
  stamp: none
  age: unknown (no stamp, and no logged date for when the gap began)
  escalate: false
  date: 2026-11-04
  finding: "The section's Source: line cites [[adr-0017-unattended-preparation-attended-decisions]] and carries no Verified: stamp beneath it, so its currency is unknown (§7). The section records an open question rather than a fact, and the contract does not exempt open-questions sections from stamps; the owner chose on 2026-11-04 to queue these rather than exempt them. The 2026-10-28 sweep did not raise them. To close: re-read the cited source and stamp the section with the method used, or remove the question if the source has since answered it."

- id: dc18c5c1
  category: unstamped fact
  status: open
  file: knowledge-architecture-open-questions.md
  section: "Would a `Recheck:` assertion have caught the stale hook fact sooner?"
  source: "2026-09-22-two-guardrail-defects-reached-the-remote-scripts-committed"
  severity: 4
  owner: "@dhtaylor"
  stamp: none
  age: unknown (no stamp, and no logged date for when the gap began)
  escalate: false
  date: 2026-11-04
  finding: "The section's Source: line cites [[2026-09-22-two-guardrail-defects-reached-the-remote-scripts-committed]] and carries no Verified: stamp beneath it, so its currency is unknown (§7). The section records an open question rather than a fact, and the contract does not exempt open-questions sections from stamps; the owner chose on 2026-11-04 to queue these rather than exempt them. The 2026-10-28 sweep did not raise them. To close: re-read the cited source and stamp the section with the method used, or remove the question if the source has since answered it."

- id: ef570338
  category: unstamped fact
  status: open
  file: knowledge-architecture-open-questions.md
  section: "Was fan-out token cost ever measured here?"
  source: "adr-0011-no-orchestrator-until-routing-fails"
  severity: 4
  owner: "@dhtaylor"
  stamp: none
  age: unknown (no stamp, and no logged date for when the gap began)
  escalate: false
  date: 2026-11-04
  finding: "The section's Source: line cites [[adr-0011-no-orchestrator-until-routing-fails]] and carries no Verified: stamp beneath it, so its currency is unknown (§7). The section records an open question rather than a fact, and the contract does not exempt open-questions sections from stamps; the owner chose on 2026-11-04 to queue these rather than exempt them. The 2026-10-28 sweep did not raise them. To close: re-read the cited source and stamp the section with the method used, or remove the question if the source has since answered it."
```
