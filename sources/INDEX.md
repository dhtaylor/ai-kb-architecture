---
name: sources-index
description: Citation registry for this domain — stubs for the artifacts its facts were folded from.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: index
  node_type: router
  created: 2026-09-19
tags: [knowledge-architecture, sources, index]
keywords: [sources, citations, stubs, provenance]
---
# Sources

- [knowledge-agent-architecture-combination-plan](knowledge-agent-architecture-combination-plan.md) — the architecture proposal this domain was folded from
- [knowledge-agent-architecture-combination-plan-revised](knowledge-agent-architecture-combination-plan-revised.md) — the plan as revised from build findings; supersedes the entry above
- [adr-0002-governance-without-enforcement](adr-0002-governance-without-enforcement.md) — engine decision: governance written in full, enforced only where enforceable
- [adr-0007-engine-content-separation](adr-0007-engine-content-separation.md) — engine decision: the engine holds no knowledge; content lives in per-domain libraries
- [adr-0008-machine-bootstrap](adr-0008-machine-bootstrap.md) — engine decision: one command registers the engine; hooks find it through git config, not a shell file
- [adr-0009-recheck-assertions](adr-0009-recheck-assertions.md) — engine decision: facts carry an assertion; the verifier reports, never repairs
- [adr-0011-no-orchestrator-until-routing-fails](adr-0011-no-orchestrator-until-routing-fails.md) — engine decision: no orchestrator built; revisit triggers
- [adr-0012-phase-4-safety-scoped-to-one-maintainer](adr-0012-phase-4-safety-scoped-to-one-maintainer.md) — engine decision: safety controls as built, declared versus enforced, dormant
- [adr-0013-steady-state-surfaced-not-scheduled](adr-0013-steady-state-surfaced-not-scheduled.md) — engine decision: cadence is surfaced when due, not run unattended
- [adr-0005-currency-cadence](adr-0005-currency-cadence.md) — engine decision: cadence; amended 2026-10-16 so a domain may declare agent_ttl
- [adr-0014-library-registry](adr-0014-library-registry.md) — engine decision: libraries are registered per machine and can live anywhere; the engine owns the library hook
- [adr-0015-releases-and-pinning](adr-0015-releases-and-pinning.md) — engine decision: semantic versioning of the engine, the two-day freeze, what a library release records; its pinning section superseded by ADR-0016
- [adr-0016-versioned-library-contract](adr-0016-versioned-library-contract.md) — engine decision: the library contract has its own MAJOR.MINOR version, a library declares it, and pinning is the library's business
- [adr-0017-unattended-preparation-attended-decisions](adr-0017-unattended-preparation-attended-decisions.md) — engine decision: prepare unattended, decide attended, report weekly; amends ADR-0013
- [adr-0019-engine-and-reference-library-release-together](adr-0019-engine-and-reference-library-release-together.md) — engine decision: this library takes the engine's release tag; the release-day steps for both repositories
- [adr-0020-published-libraries-and-consumers](adr-0020-published-libraries-and-consumers.md) — engine decision: links_to names each linked library's origin, a same-named library from elsewhere is a dependency mismatch, a consumer's copy is read-only
- [adr-0021-engine-state-at-fixed-paths](adr-0021-engine-state-at-fixed-paths.md) — engine decision: the engine's state and config are at fixed paths, XDG_STATE_HOME and XDG_CONFIG_HOME are not read, KB_PACKETS and KB_REPORTS override
- [adr-0022-evidence-not-identity-sets-the-lease](adr-0022-evidence-not-identity-sets-the-lease.md) — engine decision: the evidence behind a stamp, not who wrote it, sets its lease; unasserted_ttl at contract 2.7; check-stamps retired
- [conventions-contract-versions](conventions-contract-versions.md) — engine contract text: what each contract version adds, the scope line, attended rechecks
- [engine-changelog-v0.3.0](engine-changelog-v0.3.0.md) — engine change notes for v0.3.0, the changes after the first release, with issue numbers
- [engine-changelog-v0.4.0](engine-changelog-v0.4.0.md) — engine change notes after v0.3.0, released as v0.4.0: the read-only role, contract 2.6 and `--ref` pinning as built
- [engine-changelog-v0.5.0](engine-changelog-v0.5.0.md) — engine change notes for v0.5.0 (the changes after v0.4.0), at tag v0.5.0: fixed paths, KB_TODAY, the nudge and weekly report, kb-verify's reading of stamps
- [engine-changelog-v0.6.0](engine-changelog-v0.6.0.md) — engine change notes after v0.5.0, at tag v0.6.0: evidence sets the lease, contract 2.7, check-stamps retired, coverage in the nudge, the Attention policy and the kbengine modules
- [engine-contributing-schedulable](engine-contributing-schedulable.md) — the engine's CONTRIBUTING.md, including its section on commands safe to schedule: setting up a machine, the daily loop, troubleshooting, working on the engine, adding a library
- [engine-watcher-readme](engine-watcher-readme.md) — engine document: what the Watcher watches, and the allowlist as shipped
- [state-change-gates](state-change-gates.md) — engine document: every state-changing entry point against the three automation bands
- [script-kb-verify](script-kb-verify.md) — header of the Verifier script
- [script-kb-watch](script-kb-watch.md) — header of the Watcher script
- [script-kb-due](script-kb-due.md) — header of the cadence-nudge script
- [script-kb-prepare](script-kb-prepare.md) — header of the unattended preparation script that drafts a review packet
- [script-kb-apply](script-kb-apply.md) — header of the script that lists, shows and closes review packets
- [script-kb-report](script-kb-report.md) — header of the weekly report of decisions
- [script-kb-schedule](script-kb-schedule.md) — header of the opt-in weekly job
- [script-kb-backup](script-kb-backup.md) — header of the verified-by-restore backup of the engine and every library
- [script-kb-telemetry](script-kb-telemetry.md) — header of the retrieval cost report read from transcripts
- [script-kb-eval-sample](script-kb-eval-sample.md) — header of the seeded retrieval sample graded and joined with cost
- [script-kb-libraries](script-kb-libraries.md) — header of the registry reader and the states a library can be in
- [script-kb-library](script-kb-library.md) — header of the registration command: its overlap warning and `--ref` pinning
- [script-kb-session-start](script-kb-session-start.md) — header of the session and subagent hook
- [script-check-kb](script-check-kb.md) — the structure check's contract version gating
- [tests-run-checks](tests-run-checks.md) — preamble of the regression suite: the environment it clears before building fixtures
- [script-check-blast](script-check-blast.md) — header of the high-blast-radius gate script
- [script-kb-owner](script-kb-owner.md) — header of the CODEOWNERS resolver script
- [script-check-secrets](script-check-secrets.md) — header and output text of the three-pass secret and identifier scan
- [script-secret-entropy](script-secret-entropy.md) — header of the heuristic pass for shapeless credentials
- [script-pii-scan](script-pii-scan.md) — header of the personal-identifier pass, with its allowlist file
- [script-kb-capture](script-kb-capture.md) — header of the capture command and its refusal of secret-bearing notes
- [script-check-transient](script-check-transient.md) — header of the engine's check-transient script: dated and ticket-titled headings in fact files
- [skill-kb-distill](skill-kb-distill.md) — the engine's kb-distill skill, Steps 1 and 4: a reading is not a fact, unmerged code is not current state, supersede, subject headings
- [script-kb-sources](script-kb-sources.md) — header of the external Source URL liveness check
- [script-grade-routing](script-grade-routing.md) — header of the routing check's agent tier, graded from tool calls
- [script-grade-eval](script-grade-eval.md) — header of the answer-grounding grader, with --transcripts
- [agent-kb-retrieve](agent-kb-retrieve.md) — the retrieval agent's definition: the listed libraries are the complete set; quote what an answer rests on
- [pr-145-distill-current-state](pr-145-distill-current-state.md) — engine pull request: kb-distill writes current state, with its trap-test results
- [pr-162-unstamped-since-sweep](pr-162-unstamped-since-sweep.md) — engine pull request: the nudge names only unstamped sources new since the last sweep, read from git
- [pr-159-search-limits-per-question](pr-159-search-limits-per-question.md) — engine pull request: kb-retrieve's search limits count per question across libraries; a visible tally tried and dropped
- [pr-54-kb-retrieve-quotes](pr-54-kb-retrieve-quotes.md) — engine pull request: verbatim quotes and transcript grading, with a live sample
- [pr-6-secret-entropy-pass](pr-6-secret-entropy-pass.md) — engine pull request: the heuristic pass for shapeless credentials, and its unstated base64 limit
- [pr-8-distill-secrets-rule](pr-8-distill-secrets-rule.md) — engine pull request: reference-not-value at the distill gate, with the blind trap test
- [pr-59-archived-artifacts](pr-59-archived-artifacts.md) — engine pull request: archived raw artifacts skip only the bare-entropy rule; UTF-16 is blocked as unscanned
- [issue-5-distill-secrets-gate](issue-5-distill-secrets-gate.md) — engine issue: why consolidating a security finding is where a literal gets pasted
- [issue-9-kb-capture-scan](issue-9-kb-capture-scan.md) — engine issue and decision: capture refuses, because retrieval reads the working tree
- [issue-126-boundary-golden-cases](issue-126-boundary-golden-cases.md) — engine issue: golden cases at the boundary between neighboring libraries
- [issue-127-run-checks-git-env](issue-127-run-checks-git-env.md) — engine issue: the regression suite aimed its fixtures at the repository being committed to
- [issue-147-not-found-cost](issue-147-not-found-cost.md) — engine issue: the cost of a fact-not-found answer, and the field test of the files-first and stop rules
- [adr-0018-triage-and-specialists](adr-0018-triage-and-specialists.md) — engine decision (Proposed): a triage desk sends each question to one library's specialist; the measurement and review trigger
- [issue-130-triage-and-specialist-retrieval](issue-130-triage-and-specialist-retrieval.md) — engine issue: a proposed design for retrieval as triage plus specialists; not built
- [eval-2026-09-26-intelligence-analysis-skill](eval-2026-09-26-intelligence-analysis-skill.md) — engine evidence: the behavioural eval of a thin review skill, and the post hoc gold-key amendment
- [eval-2026-09-28-intelligence-analysis-bakeoff](eval-2026-09-28-intelligence-analysis-bakeoff.md) — engine evidence: the blind head-to-head against the legacy skill
- [eval-2026-09-28-routing-sample](eval-2026-09-28-routing-sample.md) — engine evidence: the 12-question routing sample behind ADR-0011
- [eval-2026-10-12-routing-run](eval-2026-10-12-routing-run.md) — engine evidence: the second routing sample, graded from tool calls
- [eval-2026-10-23-section-reads](eval-2026-10-23-section-reads.md) — engine evidence: the third routing sample, after section-targeted reads, and the off-KB answer it found
- [engine-readme](engine-readme.md) — the engine's README: what the repository holds, getting started, how it is used, the guardrails, status
- [adr-0001-kb-root-resolution](adr-0001-kb-root-resolution.md) — engine decision: how an agent resolves the knowledge root with no literal path; amended by ADR-0008
- [plugin-manifests](plugin-manifests.md) — the engine's marketplace manifest and the two plugin manifests
- [adr-0006-behaviour-distribution](adr-0006-behaviour-distribution.md) — engine decision: behavior travels as a plugin registered explicitly, from a directory source; amended by ADR-0007
- [skill-kb-ask](skill-kb-ask.md) — the engine's kb-ask skill, the triage desk: what it is, its tier and tools
- [skill-kb-update-domain](skill-kb-update-domain.md) — the engine's kb-update-domain skill: purpose, gate, refusals, checks
- [skill-kb-create-domain](skill-kb-create-domain.md) — the engine's kb-create-domain skill: purpose, gates, refusals, scripts
- [skill-kb-organize-domain](skill-kb-organize-domain.md) — the engine's kb-organize-domain skill: purpose, gate, refusals, checks
- [skill-kb-audit](skill-kb-audit.md) — the engine's kb-audit skill: purpose, mutations, gate, refusals, scripts
- [skill-intelligence-analysis](skill-intelligence-analysis.md) — the engine's intelligence-analysis skill in analysis-tools
- [command-kb-capture](command-kb-capture.md) — the engine's /kb-capture command
- [command-kb-init](command-kb-init.md) — the engine's /kb-init command
- [script-check-tool-scope](script-check-tool-scope.md) — docstring of check-tool-scope: the tier table and what a declaration restricts (drop if another group writes the same stub)
- [script-check-embedded-facts](script-check-embedded-facts.md) — header of the lint that scans agent and command files for embedded facts
- [script-check-exec-bits](script-check-exec-bits.md) — comment block of the executable-bit guard
- [script-check-golden](script-check-golden.md) — header of the golden-set oracle check
- [script-check-scope](script-check-scope.md) — header of the scope-attribute check
- [script-check-stamps](script-check-stamps.md) — comment block of the human-stamp guard, retired in the engine after v0.5.0 (read at v0.5.0)
- [script-check-xlinks](script-check-xlinks.md) — header of the cross-library link resolver
- [script-kb-bootstrap](script-kb-bootstrap.md) — comment block of the per-machine registration script
- [script-kb-catalog](script-kb-catalog.md) — header of the triage catalog script
- [script-kb-init](script-kb-init.md) — comment block of the project-tree scaffolder
- [scripts-kbengine-package](scripts-kbengine-package.md) — module docstrings of the shared kbengine package
- [conventions-contract-rules](conventions-contract-rules.md) — engine contract: CONVENTIONS.md sections 1 to 13, the rules behind the three contract-* topic files
- [adr-0003-scoping-topology](adr-0003-scoping-topology.md) — engine decision: the three homes for knowledge, and the nested test project
- [adr-0004-scope-attribute](adr-0004-scope-attribute.md) — engine decision: scope as required frontmatter, the swap test, what check-scope enforces, and the comparative product library
- [adr-0010-cross-library-links-checked-at-commit](adr-0010-cross-library-links-checked-at-commit.md) — engine decision: cross-library links resolved at every commit that can break one, not in CI
