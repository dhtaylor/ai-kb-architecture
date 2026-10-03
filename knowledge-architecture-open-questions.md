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
keywords: [open question, gap, not established, rollback, corrections-log, digest, cold-start, token cost, runtime enforcement, push rejection, server-side gate, ADR-0002]
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
were opened, not tokens, and the sources read state no measured cost.

Source: [[adr-0011-no-orchestrator-until-routing-fails]]

## Where does the runtime's tool-scoping behaviour belong?

The plan's phase-4 finding that a declared tool scope is enforced in some places and only a
pre-approval in others is a claim about the Claude Code runtime. It was left out of this domain and
no slug exists in claude-code-runtime for it yet, so there is nothing to link.

Source: [[adr-0012-phase-4-safety-scoped-to-one-maintainer]]

## Does a CI job that fails after the push count as the enforcement boundary, or only as detection?

[[secret-governance]] records the plan's wording: the boundary is "a server-side/CI gate (push
protection, or a CI job failing the build on any detected secret)". The engine decision record on
governance describes CI as a check that runs after a push has landed ("detection, not prevention"),
so that a real leak still needs rotation and a history rewrite. The sources read do not
say whether the second counts as the boundary the plan means, or only as the compensating control
behind it. Nothing was edited to match either.

Source: [[knowledge-agent-architecture-combination-plan]] (§5.4) · [[adr-0002-governance-without-enforcement]]

## What is the current standing of the server-side gate on the engine and on the libraries?

The only source read that states it is the engine's governance decision record, dated 2026-09-19 and
amended 2026-09-22. Its statements that a push-rejection control is unavailable and that libraries
have no CI of their own are not confirmed by the later material read in the secret-governance fold,
and this library's own contributor notes describe a CI workflow for it. No source read records a
later change, so the record was neither superseded nor contradicted here. The settings themselves
are deployment configuration and platform behaviour, and do not belong in this general library.
[[secret-governance]] keeps the general fact and points at the record without restating it.

Source: [[adr-0002-governance-without-enforcement]]
