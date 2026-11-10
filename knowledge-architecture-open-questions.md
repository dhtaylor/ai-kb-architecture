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
attended rechecks that are due, a sweep or notes awaiting distillation that no packet covers yet, while a sweep
is due the source stubs marked `Liveness: manual`, a library behind the contract or on one the engine cannot
serve, and the past week's `fact not found` runs. This library follows the script ([[unattended-preparation]]).
The difference is flagged and not resolved: no source read says which is intended.

Source: [[adr-0017-unattended-preparation-attended-decisions]] · [[script-kb-apply]] · [[script-kb-report]]
Verified: 2026-11-09 · by: agent · method: doc-review
