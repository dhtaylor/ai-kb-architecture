---
name: knowledge-architecture-open-questions
description: Open questions from the knowledge-architecture domain — things the folded sources did not establish, recorded rather than filled.
memory_type: reference
domain: knowledge-architecture
scope: general
verified: unknown
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

## Were the machine-readable bot label, the retrieval-check diff annotation, the pull notification on rollback and the per-domain corrections-log ever built?

The plan specifies each. The sources read (decisions, the state-change inventory, script headers)
record the high-blast gate, its label and the dormant two-reviewer rule, and are silent on these four.
A live check on 2026-10-02 found no `corrections-log.md` in any installed library, which says nothing
about whether the other three exist.

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§5A)

## Does the CI cold-start advice (shallow clone at a pinned tag, sparse checkout) carry over to per-domain library repositories?

The plan's text was written for one general repository. Content now lives in one repository per
domain, and no source read records a cold-start mechanism. Whether the advice still applies, or has
become obsolete as other plan machinery did, is not stated.

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§5A) · [[adr-0007-engine-content-separation]]

## Was fan-out token cost ever measured here?

The 3–15× and ~20× figures are the plan's. The routing sample measured accuracy and which libraries
were opened, not tokens, and the sources read state no measured cost. `kb-telemetry` now measures the
cost of single-agent retrieval from transcripts ([[retrieval-eval-integrity]]); no source read records a
figure from it, and no fan-out is built to measure.

Source: [[adr-0011-no-orchestrator-until-routing-fails]] · [[script-kb-telemetry]]

## Where does the runtime's tool-scoping behaviour belong?

The plan's phase-4 finding that a declared tool scope is enforced in some places and only a
pre-approval in others is a claim about the Claude Code runtime. It was left out of this domain and
no slug exists in claude-code-runtime for it yet, so there is nothing to link.

Source: [[adr-0012-phase-4-safety-scoped-to-one-maintainer]] · [[knowledge-agent-architecture-combination-plan-revised]] (§9.18)

## Would a `Recheck:` assertion have caught the stale hook fact sooner?

[[guardrail-verification]] records that a clean `kb-audit` sweep passed over a fact quoting a hook body
rewritten two commits earlier, and that the defect was found by reading the hook. The session note that
recorded it states that adding `Recheck:` assertions to more facts "would probably have caught the stale hook
fact sooner" and labels that a hypothesis, since the assertion that catches the fact today was written after
the fact was already wrong. It was left out of the fact as a claim and no source read tests it.

Source: [[2026-09-22-two-guardrail-defects-reached-the-remote-scripts-committed]]

## Where does the runtime's behaviour on library discovery by a sub-agent belong?

The contract (section 3 of `ai-kb:CONVENTIONS.md`) says the session-start hook "and the SubagentStart hook for kb-retrieve" list each registered library, and an agent given no list refuses rather than searching. Whether sub-agents receive session-start context is a claim about the Claude Code runtime. It was left out of [[retrieval-eval-integrity]], which records only the general failure, and no slug exists in claude-code-runtime for it, so there is nothing to link.

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§9.14) · [[agent-kb-retrieve]]

## Does an unneeded content read count against the routing-accuracy reopen trigger?

[[agent-lifecycle-roles]] records the routing-accuracy trigger to reopen the no-orchestrator decision. The 2026-10-12 run read the expected file in 12 of 12 cases and scored 11/12,
because one case read content from libraries it did not need. Whether that case is a routing-accuracy miss
for the trigger, which would put the run at 92%, is not stated by any source read.

Source: [[eval-2026-10-12-routing-run]] · [[adr-0011-no-orchestrator-until-routing-fails]]

## Are the contract's conformance suite and the split of its engine-facing text built?

The decision that versioned the contract also planned a named, published conformance suite per contract
version, and a split of the contract so it holds only the library-facing spec, with the engine-facing
parts (how skills conform, the session-start hook, precedence over the design plan) moved to engine
documents. No source read records either as built. The regression suite holds fixture libraries, and the
contract text read still carries its skill-rename table and its hook resolution mechanism.
The same decision specifies that a library on the previous major is warned ahead of time that its major
leaves support when the next is published; no source read records a script that gives that warning.

Source: [[adr-0016-versioned-library-contract]] · [[conventions-contract-versions]]

## Does the scope line separate neighboring libraries in practice?

The `Not covered here:` line (contract 2.3) is read by the retrieval agent to rule out a library
([[retrieval-agent-behavior]]). The sources read state the intent and the rule, and record no measurement
of whether it separates neighbors. A routing retest run after the line was introduced is recorded only in
[[2026-10-26-kb-retrieve-routing-retest-after-grader-and-scope-fixes]], an episodic note that has not been
distilled, so what it measured is not served here as a fact.

Source: [[conventions-contract-versions]] · [[script-kb-library]]

## Is retrieval below the reopen line of the no-orchestrator decision, and what replaces one agent doing triage and specialist work?

The reopen line of the no-orchestrator decision is in [[agent-lifecycle-roles]], "Orchestrator cost bounds".
Whether that line has been crossed is not established by any distilled source; the retest above is not
distilled. One direction is proposed, and it is not built and not decided: retrieval as triage plus specialists. A triage
step would read a compact catalog (each library's name, scope and `Not covered here:` line) instead of every
router and name the likeliest library, or two if the question spans them. Each specialist would be confined to
one library and either answer under the full retrieval contract or reply that the question is not its own.
A merge step would keep a single answer, flag an overlap without picking a side, and pass citations through
unchanged. No decision record supersedes the no-orchestrator decision in the sources read.

Source: [[issue-130-triage-and-specialist-retrieval]] · [[adr-0011-no-orchestrator-until-routing-fails]]

## Does the redaction filter cover what the scan cannot see?

The filter that cleans a review packet before it is saved runs the same three passes as the secret scan
([[secret-governance]]). That file records what the scan cannot see, among it a personal name in prose.
No source read says whether a packet is checked for such a name other than by the person who reads it.

Source: [[script-check-secrets]] · [[adr-0017-unattended-preparation-attended-decisions]]

## Does `kb-apply` change a fact file, and which list does the weekly report hold?

ADR-0017 says that after a person decides, `kb-apply` changes a fact file, runs the checks and commits. The
script's own header says it never writes to a library: it lists packets, shows one and closes one, and the
skill does the writing once a person has decided each item. The two also list different things for the weekly
report. The decision names packets waiting, failed rechecks, drifted or dead sources, queue items older than
the last report, and a library behind the contract. The script lists a packet waiting, each open queue item,
attended rechecks that are due, a sweep or notes awaiting distillation that no packet covers yet, and a library
behind the contract or on one the engine cannot serve. This library follows the script
([[unattended-preparation]]). The difference is flagged and not resolved: no source read says which is intended.

Source: [[adr-0017-unattended-preparation-attended-decisions]] · [[script-kb-apply]] · [[script-kb-report]]
