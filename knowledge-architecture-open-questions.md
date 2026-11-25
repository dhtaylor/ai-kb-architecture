---
name: knowledge-architecture-open-questions
description: Open questions from the knowledge-architecture domain — things the folded sources did not establish, recorded rather than filled.
memory_type: reference
domain: knowledge-architecture
scope: general
verified: 2026-11-09
metadata:
  type: reference
  node_type: open-questions
  created: 2026-10-02
tags: [knowledge-architecture, open-questions]
keywords: [open question, gap, kb-apply, kb-report, weekly report, not established, reopen trigger, rollback, corrections-log, digest, cold-start, token cost, runtime enforcement, push rejection, server-side gate, ADR-0002, sub-agent, library discovery, Recheck, stale hook fact, conformance suite, contract split, triage, specialist retrieval, scope line, routing retest, redaction, telemetry]
---
# Open questions

## Is a human-maintained digest of external changes actually kept?

[[agent-lifecycle-roles]] records the plan's low-tech default for the Watcher: a human-maintained
digest reviewed on a cadence. None of the sources read in the revised-plan fold says whether one
exists, only that the offline snapshot comparison does.

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§5A)
Verified: 2026-11-09 · by: agent · method: doc-review

## Were the machine-readable bot label, the retrieval-check diff annotation, the pull notification on rollback and the per-domain corrections-log ever built?

The plan specifies each. The sources read (decisions, the state-change inventory, script headers)
record the high-blast gate, its label and the dormant two-reviewer rule, and are silent on these four.
Live checks on 2026-10-02 and 2026-11-09 found no `corrections-log.md` in any installed library, which says
nothing about whether the other three exist. The engine's CI checks one label, `owner-approved`, on the
high-blast gate.

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§5A)
Verified: 2026-11-09 · by: agent · method: doc-review

## Does the CI cold-start advice (shallow clone at a pinned tag, sparse checkout) carry over to per-domain library repositories?

The plan's text was written for one general repository. Content now lives in one repository per
domain. Part of the advice carries over: the engine's CI template for a library clones the engine with
`--depth 1` at a release tag. A sparse checkout has nothing to narrow in a repository that holds one
domain. The template sets no cache key, and no source read says whether one is needed.

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§5A) · [[adr-0007-engine-content-separation]] · ai-kb:documents/library-guardrails.yml@5fb833a
Verified: 2026-11-09 · by: agent · method: doc-review

## Was fan-out token cost ever measured here?

The 3–15× and ~20× figures are the plan's, for a broadcast to several workers. The 2026-09-28 routing
sample measured accuracy and which libraries were opened, not tokens. ADR-0018 measured tokens and cost per
question for one agent and for the triage desk ([[retrieval-agent-behavior]]), but the desk asks one
specialist, two at most, and never broadcasts. No source read records a measured cost for a broadcast,
and none is built to measure.

Source: [[adr-0011-no-orchestrator-until-routing-fails]] · [[adr-0018-triage-and-specialists]] · [[script-kb-telemetry]]
Verified: 2026-11-09 · by: agent · method: doc-review

## Where does the runtime's tool-scoping behaviour belong?

The plan's phase-4 finding that a declared tool scope is enforced in some places and only a
pre-approval in others is a claim about the Claude Code runtime. It was left out of this domain. Its home is
a claude-code-runtime library, which is not published with this one, so this library names it in plain text
rather than linking; and no section there holds the finding yet.

Source: [[adr-0012-phase-4-safety-scoped-to-one-maintainer]] · [[knowledge-agent-architecture-combination-plan-revised]] (§9.18)
Verified: 2026-11-09 · by: agent · method: doc-review

## Would a `Recheck:` assertion have caught the stale hook fact sooner?

[[guardrail-verification]] records that a clean `kb-audit` sweep passed over a fact quoting a hook body
rewritten two commits earlier, and that the defect was found by reading the hook. The session note that
recorded it states that adding `Recheck:` assertions to more facts "would probably have caught the stale hook
fact sooner" and labels that a hypothesis, since the assertion that catches the fact today was written after
the fact was already wrong. It was left out of the fact as a claim and no source read tests it.

Source: [[2026-09-22-two-guardrail-defects-reached-the-remote-scripts-committed]]
Verified: 2026-11-09 · by: agent · method: doc-review

## Where does the runtime's behaviour on library discovery by a sub-agent belong?

The contract (section 3 of `ai-kb:CONVENTIONS.md`) says the session-start hook "and the SubagentStart hook for kb-retrieve" list each registered library, and an agent given no list refuses rather than searching. [[retrieval-agent-behavior]] states the engine's premise as its hook records it, that "a subagent never sees the session-start context". Whether and when the Claude Code runtime passes session-start context to a sub-agent is a claim about the runtime itself. Its home is a claude-code-runtime library, which is not published with this one and holds no section on it yet.

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§9.14) · [[agent-kb-retrieve]] · [[script-kb-session-start]]
Verified: 2026-11-09 · by: agent · method: doc-review

## Are the contract's conformance suite and the split of its engine-facing text built?

The decision that versioned the contract also planned a named, published conformance suite per contract
version, and a split of the contract so it holds only the library-facing spec, with the engine-facing
parts (how skills conform, the session-start hook, precedence over the design plan) moved to engine
documents. No source read records either as built. The regression suite holds fixture libraries, and the
contract text read still carries its skill-rename table and its hook resolution mechanism.
The same decision specifies that a library on the previous major is warned ahead of time that its major
leaves support when the next is published; no source read records a script that gives that warning.

Source: [[adr-0016-versioned-library-contract]] · [[conventions-contract-versions]]
Verified: 2026-11-09 · by: agent · method: doc-review

## Does the scope line separate neighboring libraries in practice?

The `Not covered here:` line (contract 2.3) is read by the retrieval agent to rule out a library
([[retrieval-agent-behavior]]). One measurement bears on it: in a field A/B, naming an absent topic in the line
cut a question about that topic "from 16-23 turns to 3" ([[retrieval-eval-integrity]]). That tests a library
ruling a question out, not two neighbors being told apart. ADR-0018's routing measurement used decoy
libraries that carry no such line, and its rerun with decoys that do is the pending test of separation
(below). The 2026-10-26 retest's scores were not distilled: they are one run's readings.

Source: [[conventions-contract-versions]] · [[script-kb-telemetry]] · [[adr-0018-triage-and-specialists]]
Verified: 2026-11-09 · by: agent · method: doc-review

## Will triage and specialists meet the review trigger that accepts them?

This replaces, on 2026-11-09, the question whether retrieval was below the no-orchestrator decision's reopen line and what would replace one agent: ADR-0018 records that the line was crossed, and built a triage desk and specialists ([[retrieval-agent-behavior]]). It is **Proposed**. Its delivery plan still lists a rerun "with decoys that carry a `Not covered here:` line, at 6 and 48 libraries", and a measure of the entry point in normal use, where a rise in direct `kb-retrieve` runs "would show sessions bypassing the desk." No source read records either as done, so whether `kb-ask` routes at least 95% at 48 libraries with no answer from the wrong library is not established.

Source: [[adr-0018-triage-and-specialists]]
Verified: 2026-11-09 · by: agent · method: doc-review

## Does the redaction filter cover what the scan cannot see?

The filter that cleans a review packet before it is saved runs the same three passes as the secret scan
([[secret-governance]]). That file records what the scan cannot see, among it a personal name in prose.
No source read says whether a packet is checked for such a name other than by the person who reads it.

Source: [[script-check-secrets]] · [[adr-0017-unattended-preparation-attended-decisions]]
Verified: 2026-11-09 · by: agent · method: doc-review

## Does `kb-apply` change a fact file, and which list does the weekly report hold?

ADR-0017 says that after a person decides, `kb-apply` changes a fact file, runs the checks and commits. The
script's own header says it never writes to a library: it lists packets, shows one and closes one, and the
skill does the writing once a person has decided each item. The two also list different things for the weekly
report. The decision names packets waiting, failed rechecks, drifted or dead sources, queue items older than
the last report, and a library behind the contract. The script lists a packet waiting, each open queue item,
how many queue records it cannot read (with the note that `check-kb` names the lines; #204),
attended rechecks that are due, a sweep or notes awaiting distillation that no packet covers yet, while a sweep
is due the source stubs marked `Liveness: manual`, ahead of the engine's contract (#204),
or on one the engine cannot serve (a library behind no longer is, #263), and the past week's `fact not found` runs. This library follows the script ([[unattended-preparation]]).
The difference is flagged and not resolved: no source read says which is intended.

Source: [[adr-0017-unattended-preparation-attended-decisions]] · [[script-kb-apply]] · [[script-kb-report]] · [[engine-changelog-v0.5.0]] · [[engine-changelog-v0.6.0]] (#263)
Verified: 2026-11-24 · by: agent · method: doc-review

## Which variables does the regression suite clear: the ones the change notes name, or every declared one?

The change notes for #181 say `tests/run-checks` clears `KB_LIBRARY_HOME`, `KB_ENGINE_ROOT`, `KB_SYSTEMCTL`, `KB_CRONTAB` and some of git's variables. ADR-0021 says "the test suite can clear the declared list instead of learning each one from the bug that it caused", where the declared list is every variable `kbengine.env` declares. Neither says which the suite does; [[guardrail-verification]] records the change notes' list and no more.

Source: [[engine-changelog-v0.5.0]] · [[adr-0021-engine-state-at-fixed-paths]]
Verified: 2026-11-20 · by: agent · method: doc-review

## What forms of the day does `kb.nudge` accept in `weekly:<day>`?

[[agent-lifecycle-roles]] records the setting as `weekly:<day>` and that a value that is not one of `always`, `weekly:<day>` and `off` warns and acts as `always`. CONTRIBUTING says the day is `mon`..`sun`, 'or the full name, any case', and that the nudge shows 'on that local weekday'. The change notes do not say which timezone 'local' means beyond that; [[using-ai-kb]] records the day forms.

Source: [[engine-changelog-v0.5.0]] · [[engine-contributing-schedulable]]
Verified: 2026-11-20 · by: agent · method: doc-review

## Does `check-xlinks` say the origin cannot be confirmed for a local-only library?

[[releases-and-contract-versioning]] records, for a library registered with no origin, that its links are resolved as before "plus a note that the origin cannot be confirmed", and, for a local-only library listed by its path, that `check-xlinks` "compares nothing and resolves the links as for any library". The sources read do not say whether the note is printed in the second case.

Source: [[conventions-contract-versions]] · [[adr-0020-published-libraries-and-consumers]]
Verified: 2026-11-20 · by: agent · method: doc-review

## Is a CHANGELOG entry required for every change to the engine, or only for a new contract version?

CONTRIBUTING says that "`CHANGELOG.md` records the change under `## Unreleased`" in its bullet on a new contract version, and says releases take their notes from `CHANGELOG.md`. It does not say whether every other change needs an entry.

Source: [[engine-contributing-schedulable]] · [[using-ai-kb]]
Verified: 2026-11-21 · by: agent · method: doc-review

## Are the three open items ADR-0001 lists still open?

ADR-0001 lists three, dated 2026-09-19 and before its amendment: the commit-SHA divergence warning for the root is not implemented; the bootstrap command does not exist; and two failure branches of the session hook were verified only by pipe test. The bootstrap command exists since ADR-0008. Neither source says whether the divergence warning was built or the failure branches were since observed.

Source: [[adr-0001-kb-root-resolution]] · [[adr-0008-machine-bootstrap]]
Verified: 2026-11-21 · by: agent · method: doc-review

## Does `kb-bootstrap` with no flag exit 0 when nothing is out of step?

CONTRIBUTING says the no-flag form "exits 1 if anything is out of step". ADR-0008's tests record exit 0 after an apply for `--check`. No source states the no-flag exit code when everything is in step, or what exit code means a failure to run.

Source: [[engine-contributing-schedulable]] · [[adr-0008-machine-bootstrap]]
Verified: 2026-11-21 · by: agent · method: doc-review

## Does a library clone on a machine need `kb-library add`, or does `kb-init` also wire it?

CONTRIBUTING says `kb-library add <path>` wires a library clone to the engine's hook, and says `kb-init` is run "in every clone" of a project. Neither source says what a person does for a second clone of a library on a machine where the library is already registered.

Source: [[engine-contributing-schedulable]]
Verified: 2026-11-21 · by: agent · method: doc-review

## Which ADR-0006 statements does ADR-0007 supersede?

ADR-0006's status line reads "amended ... by ADR-0007", and ADR-0007 says ADR-0006's inclusion of the facts in the knowledge repository "is the problem". Neither document lists which of ADR-0006's other statements, such as its table of files that moved in, still stand. [[skills-agents-and-commands]] folds only the plugin-distribution decision and its amendment.

Source: [[adr-0006-behaviour-distribution]] · [[adr-0007-engine-content-separation]]
Verified: 2026-11-21 · by: agent · method: doc-review

## Does ADR-0006's per-project registration snippet still describe how a machine is set up?

ADR-0006 shows `extraKnownMarketplaces` and `enabledPlugins` written in a consumer's project settings, with `"path": "./knowledge"`. The ADR-0008 text read for this fold says the machine bootstrap writes "the marketplace and plugin entries" in `~/.claude/settings.json`. No source read says whether the per-project snippet is still used or was replaced.

Source: [[adr-0006-behaviour-distribution]] · [[adr-0008-machine-bootstrap]]
Verified: 2026-11-21 · by: agent · method: doc-review

## Was Claude Code's directory-discovery walk ever observed?

ADR-0006 marks the discovery behavior "Documented, not verified here" and says Phase 3 would test it. Its later verification note closes two other questions (the relative path and read-in-place) and does not mention the walk.

Source: [[adr-0006-behaviour-distribution]]
Verified: 2026-11-21 · by: agent · method: doc-review

## Does kb-audit stop for a person before it commits, outside prepare mode?

kb-audit's Steps 1 to 5 write the queue and `last_swept` and name no review stop; the stop and approval appear only in "Resuming from a review packet" ("commit only on approval"). The sources read do not say whether an attended sweep commits without one.

Source: [[skill-kb-audit]]
Verified: 2026-11-21 · by: agent · method: doc-review

## Which file holds the `verification` tier?

check-tool-scope lists a `verification` tier and says "nothing in the current behaviour layer holds this tier". No source read says what is intended to hold it.

Source: [[script-check-tool-scope]]
Verified: 2026-11-21 · by: agent · method: doc-review

## Which default folders does check-embedded-facts scan?

The header says "default: .claude/agents/, .claude/commands/". Run at engine commit 5433f08, with no argument the script scanned plugins/*/skills, plugins/*/agents and plugins/*/commands, and a finding in .claude/agents/ was not scanned. The sources do not say which is intended.

Source: [[script-check-embedded-facts]]
Verified: 2026-11-21 · by: agent · method: doc-review

## Do the headers of check-kb, check-golden and check-scope omit a usage-error exit code on purpose?

Each header states "Exit 0 clean, 1 on any defect." Run at 5433f08, each exits 2 with a usage line when given no argument. Whether exit 2 is part of the contract the headers should state is not established.

Source: [[script-check-kb]] · [[script-check-golden]] · [[script-check-scope]]
Verified: 2026-11-21 · by: agent · method: doc-review

## Are the exit codes of scripts whose headers state none a contract?

The headers of check-exec-bits, check-stamps (partly), kb-init, kb-capture, kb-session-start, pii-scan and secret-entropy state no exit codes or no usage-error code. The codes recorded in the script reference were observed by running them. The sources do not say whether callers may rely on them.

Source: [[script-check-exec-bits]] · [[script-kb-init]] · [[script-kb-capture]] · [[script-kb-session-start]] · [[script-pii-scan]] · [[script-secret-entropy]]
Verified: 2026-11-21 · by: agent · method: doc-review

## Which parts of ADR-0003's table of homes still apply after the library model?

ADR-0003 names the general tier as `$KB_GENERAL_ROOT/knowledge/` in a workspace repository, and its status line says it was amended by ADR-0006 and ADR-0007. Section 1 of the contract now places the general home in a domain library, registered per machine. Neither the contract nor ADR-0003 (as read) says which rows or paths of ADR-0003's table were superseded; ADR-0006 was not read in this fold.

Source: [[adr-0003-scoping-topology]] · [[conventions-contract-rules]] (§1)
Verified: 2026-11-21 · by: agent · method: doc-review

## Which design plan does the contract's precedence rule name?

The contract says "the combination plan is the design authority" and that when the contract and the plan disagree, the plan wins. This library holds two versions of that plan, the original and the revised one. The contract text read here does not say which one it means, or whether the revised plan replaced the original as the authority.

Source: [[conventions-contract-rules]] (the precedence paragraph) · [[knowledge-agent-architecture-combination-plan-revised]]
Verified: 2026-11-21 · by: agent · method: doc-review

## Does anything fail a Recheck that greps a bare title or subject longer than four words?

The contract says `kb-verify` reports a fetched fragment of four words or fewer, and a (url, fragment) pair backing more than two sections or sections in more than one file, and that "a stamp the assertion backs is no stronger than the assertion". Nothing read says whether any check fails, rather than reports, an assertion that greps a title or subject longer than four words. Since ADR-0022 the reported findings have a consequence: a flagged assertion is demoted to an unasserted stamp ([[agent-lifecycle-roles]] § A stamp's lease follows its evidence, not its author). The lint still flags only the three kinds it names, so a title or subject longer than four words is not flagged, and so is not demoted.

Source: [[conventions-contract-rules]] (§7) · [[adr-0022-evidence-not-identity-sets-the-lease]]
Verified: 2026-11-24 · by: agent · method: doc-review
