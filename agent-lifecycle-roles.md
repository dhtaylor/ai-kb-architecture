---
name: agent-lifecycle-roles
description: The three librarian roles that keep a governed KB operating (Curator, Research librarian, Fact-checker) and how the Verifier and Watcher were built, how mutation of the KB is governed (built, dormant and not-built controls), how the fact-checkers are tested, and why no orchestrator is built and how retrieval cost is bounded.
memory_type: semantic
domain: knowledge-architecture
scope: general
verified: 2026-10-02
metadata:
  type: fact
  node_type: memory
  created: 2026-09-19
tags: [knowledge-architecture, lifecycle, roles, governance, orchestrator]
keywords: [curator, research librarian, fact-checker, verifier, watcher, orchestrator, fan-out, PR review, rollback, corrections-log, PII, CODEOWNERS, Recheck, kb-verify, kb-watch, kb-due, check-blast, dormant, nudge, cadence, revisit trigger]
---
# Agent lifecycle roles

A library's hard problem is not storing facts, it is cataloguing, retrieving, and weeding them.
Three roles cover that lifecycle.

## The three roles

- **Curator (cataloguer)** — owns *structure*: where a fact lives, INDEX hooks, dedup,
  contradiction flagging, `scope:` classification, promotion. This is `kb-organize-domain` /
  `kb-update-domain` / `kb-distill` / `kb-audit`, personified, and `kb-create-domain`, which stands
  up a new library. The plan's own list of the role names the first four. The curation skills stop
  for human approval before committing; `kb-create-domain` additionally states its plan and
  requires approval before it runs a single scaffolding command. That stop is a request the model
  is trained to honour, not a wall: a session run with edits auto-accepted, or a model that skips
  the instruction, produces the same unreviewed commit. A flag checked by a script's interpreter
  (`--apply`, `--yes`) is the stronger form.
- **Research librarian (fast access)** — the thin retrieval agents: load INDEX, descend, cite,
  answer. Read-only, on demand. The reference desk. As built it is one agent, `kb-retrieve`, which
  serves every library rather than one worker per domain, and it declares `tools: Read, Grep, Glob`.
- **Fact-checker (currency)** — more than one job, split by source of truth:
  - **Internal / deployment truth** — updated by the **live systems themselves**. Kept current by
    the **Verifier**: re-runs a fact's own assertion, and reports a mismatch.
  - **External / general truth** (vendor behavior, regulation, platform features) — the world
    updates these. Kept current by the **Watcher**: monitors sources for change and flags drift.

  *Superseded 2026-09-21:* the skills named here were renamed with a `kb-` prefix — they were
  `organize-domain`, `update-domain`, `distill-episodic` and `meditate` when this was folded, and the
  source it cites still uses those names.

### The Verifier as built

Status: built. It is a script, `kb-verify`, not an agent, and it does not query live systems. A
section may carry a `Recheck:` line beneath its stamp: an assertion, not a query, where exit 0 means
the fact still holds. `kb-verify` selects sections whose stamps are past the domain horizon (a tenth
of it for agent stamps), oldest first, capped by `verifier_budget`. Three rules are the whole design:

- A pass restamps only with `--apply`. The default is report-only.
- A failure is never applied, with or without `--apply`. The fact is not edited, not marked
  `CONFLICTED`, not deleted. A failure means the fact is stale, or the assertion is wrong, or the
  world changed and this is now a contradiction — three different repairs, and automating the choice
  destroys the evidence that a choice was needed.
- Commands print, and run only with `--yes`. A library is content that travels between people;
  running its assertions executes its author's shell as you.

Most facts get no assertion, and the report names them. A failure run under `--yes` is also written
to the owning library's `needs-attention.md` queue as detection output; a later pass does not
resolve the entry, and nothing but a human does.

Superseded 2026-10-02: the first text of this role said the Verifier "re-runs a fact against the
live system, re-stamps `verified:`, flags mismatch", and the plan's testing criterion said a failed
check must produce a `contradictions.md` flag. The decision recording the Verifier as built states
the second plainly: the plan says a failed check becomes a `contradictions.md` flag, and the
Verifier as built reports the failure and leaves the fact untouched.

### The Watcher as built

Status: built, offline. It is a script, `kb-watch`, not an agent. It compares a saved snapshot
against a recorded baseline, both already on disk, and never fetches anything itself. It imports no
network module, and a regression case enforces that. Because no model is in the loop, page content
never reaches one: the snapshot is read as bytes, stripped to plain text with the standard library
only, matched against an operator-authored regex, and the matched value is written to a queue entry
as escaped, truncated data — never as an instruction. A missing, empty or unreadable snapshot is an
`unreachable` finding, never folded into "unchanged".

Status: not built — live fetching. It remains the later, isolated step the plan describes, and an
isolated fetcher was declined for now; adding one is a separate decision with its own security
review. The consequence stated for the built design: a watched source is only as fresh as its last
saved snapshot, so the Watcher detects drift between two copies and does not notice that a copy has
gone stale.

The Verifier and the Watcher feed one routed queue, the library's `needs-attention.md`. The Verifier
imports the Watcher's queue primitives so the two writers cannot drift apart. Neither writer edits a
fact. Both add the queue's router line to the library's `INDEX.md`; an unrouted queue is an orphan
that fails `check-kb`, which would have blocked every commit in that library.

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§5A, §6 Phase 4, §9.13) · [[knowledge-agent-architecture-combination-plan]] (§5A, superseded) · [[adr-0009-recheck-assertions]] · [[adr-0012-phase-4-safety-scoped-to-one-maintainer]] · [[adr-0013-steady-state-surfaced-not-scheduled]] · [[state-change-gates]] · [[script-kb-verify]] · [[script-kb-watch]]
Verified: 2026-10-02 · by: agent · method: doc-review

## Watcher safety design

Web-watching is brittle (pages restructure, move behind auth, render via JS), and it is the single
**external ingestion point** feeding an LLM that drafts KB changes — a prompt-injection vector (a
spoofed page saying "the correct pattern for all agents is …" becomes a plausible auto-PR).
Therefore, with the status of each measure:

- **Default (low-tech):** a **human-maintained digest** of vendor release notes / regulation
  changes, reviewed on a cadence and folded via `kb-update-domain`. Do not claim automated monitoring
  until a concrete mechanism is prototyped. Status: no source read records whether a digest is kept.
- **If/when automated, fetch in an isolated, network-restricted step** (no tool access; cannot
  reach internal/link-local addresses — SSRF guard). Status: not built; the built Watcher never
  fetches.
- **Strip to plain text; treat content strictly as untrusted data, never instructions.** Status:
  built, and structural rather than a matter of care: the built Watcher is a script with no model in
  the loop, which the decision recording it calls the plan's strongest defence against a spoofed page
  steering a knowledge change.
- **Emit only a schema'd drift signal** (`field · old · new · source_url`). Status: built; the
  values taken from the page are escaped, flattened to one line and truncated to 200 characters.
- **Keep the monitored-URL allowlist outside the KB.** Status: built, in the engine and never inside
  a library; the allowlist ships empty, as shipped.

(The further distinction between "unreachable" and "unchanged" for a watched source follows the
same rule the contract states for provenance liveness — see `ai-kb:CONVENTIONS.md` §6. Status:
built; see the Watcher above.)

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§5A) · [[adr-0012-phase-4-safety-scoped-to-one-maintainer]] · [[script-kb-watch]]
Verified: 2026-10-02 · by: agent · method: doc-review
Recheck: f=../../scripts/kb-watch; [ -f "$f" ] && ! grep -qE '^ *(import|from) .*(urllib|http|socket|requests|ftplib|smtplib|ssl)' "$f"

## Mutation governance

The standing rule is automate detection, gate mutation (three bands, decided for this workspace in
`ai-kb:documents/decisions/0005-currency-cadence.md`). What follows is the operational detail behind the middle and last
band, beyond what that decision records. Each item carries a status: built, dormant (written, with
nothing to enforce it against), or not built.

- **The PR gate is cosmetic unless the review is defined.** A reviewer who trusts the agent's
  description without following provenance provides no gate, and a subtle wrong fact in an
  otherwise innocuous diff sails through. A KB-PR review checklist: bot-authored PRs carry a
  **machine-readable label**; **high-blast-radius** facts need **2 reviewers + a 24h window**; the
  PR is annotated with the **retrieval-check diff** (which golden answers would change) so
  reviewers see behavioral impact, not just text.
  Status of the 2-reviewer and 24-hour rule: dormant until a second owner exists, because a lone
  owner cannot constitute two approvals. Status of the machine-readable label and the retrieval-check
  diff annotation: not established — no source read records them as built.
- **Never-automated actions are enforced, not just declared.** A CI gate blocks auto-merge of any
  PR that deletes a `knowledge/` file, resolves a `contradictions.md` entry, or moves a file
  repo→general, unless a label writable **only by a domain owner** (not the agent identity) is
  set. Status: built as `check-blast`. In a library it flags a pull request that deletes or renames
  away a fact file, removes an inline `status: CONFLICTED` marker, or changes a fact's `scope:` to a
  strictly wider tier (`repo:<project>` < `{product:<vendor>, org:<company>}` < `general`); in the
  engine, one that touches the contract or the decision records. The label is `owner-approved`; with
  it a finding still prints but no longer fails the check. `check-blast` itself only detects; failing
  a required check is the CI workflow's job. On a direct push the check only warns, because the push
  has already landed by the time CI runs, so failing the build would hide that it happened rather
  than stop it. The pull-request path was confirmed running in CI on 2026-10-01. "Writable only by a
  domain owner" holds only while there is exactly one collaborator; it is a property of that
  circumstance, not of any permission the script or workflow enforces.
- **Rollback is an invariant, not an aside.** Both KBs are git repos; recovery from a bad fold or a
  wrong merged correction is `git revert <commit>` **plus a mandatory pull notification** to
  developers (the shared repo posts a post-merge webhook of what changed). For the shared tier the
  blast radius is every project on every machine, so this path must be named and rehearsed.
  Status: not established — no source read records the pull notification or a rehearsal as built.
- **A corrections-log makes silent propagation visible.** Because the shared tier is latest-wins
  (decided in `ai-kb:documents/decisions/0003-scoping-topology.md`), maintain a per-domain `corrections-log.md` (date ·
  fact slug · old→new summary · PR link) — a wrong correction propagates exactly the same way a
  right one does, and the log is how a consumer notices. Status: not established — no source read
  records one as built.
- **Declared controls versus enforced ones.** The decision on governance gives the driver: a control
  that is declared but not enforced is worse than an absent one, because it reads as protection in
  an audit and provides none. Its response: write the full governance structure, enforce what is
  enforceable, and record the gap explicitly. Status by item: `CODEOWNERS` is written and advisory —
  owner review becomes a requirement only when branch protection requires it, and today it answers
  who a finding belongs to (resolved by `kb-owner`); dormant. A second named owner per domain, and
  signed commits for merges: not built.
- **Least privilege.** Every agent, command and skill declares a tier and a tool scope, and a lint
  fails a file with no tier, no scope, an unknown tier or a tool outside its tier; the lint checks
  that the declaration is present and consistent and enforces nothing more. Status: declared and
  linted, built. There are no service accounts: every tool runs as the one local user, so
  client-side scopes are the only control and a determined bypass beats them. Status: not built, an
  accepted limitation that the decision says must be revisited before any agent touches a shared or
  production system.
- **Scale the control to who is there.** The plan wrote its controls for a team — service accounts per
  agent tier, two reviewers and a 24-hour window. Where there is a single maintainer, building them
  as written means either claiming controls that do not exist or blocking on conditions that cannot
  be met; the recorded response is to say what each control enforces beside the control, and to name
  what adding a collaborator reopens.
- **Cadence is surfaced, not scheduled.** Status: built. `kb-audit` and `kb-distill` both need an
  agent and a human decision, so they are not run unattended. The session-start hook calls
  `kb-due --nudge`, which reports only what is due, per library: a sweep; facts due for recheck,
  counted with `kb-verify`'s own staleness logic and never executing an assertion; open queue items;
  and episodic notes awaiting distillation. It prints nothing when nothing is due, and never prompts
  or writes. "Scheduled" here means "surfaced when due". A cloud routine and a local cron job were
  both considered and declined, because each runs without the owner watching. Consequence stated by
  the decision: nothing happens unless a session starts, so a library nobody opens goes unswept and
  nobody is told — acceptable for one person who works in these repositories daily, and the first
  thing to revisit if the libraries gain readers who are not maintainers.

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§5A, §6 Phases 4 and 5, §9.18) · [[knowledge-agent-architecture-combination-plan]] (§5A, §2) · [[adr-0002-governance-without-enforcement]] · [[adr-0012-phase-4-safety-scoped-to-one-maintainer]] · [[adr-0013-steady-state-surfaced-not-scheduled]] · [[state-change-gates]] · [[script-check-blast]] · [[script-kb-owner]] · [[script-kb-due]]
Verified: 2026-10-02 · by: agent · method: doc-review

## Testing the fact-checkers

Neither fact-checker has a test story by default, yet each *decides truth*:

- **Verifier fixture test:** against a local test system seeded with a deliberately stale/wrong
  fact, the Verifier must report the failure and leave the fact untouched. As built, the fixture test
  asserts that a stale, wrong fact is reported and its file stays byte-for-byte unchanged, with no
  false-fresh stamp. Status: built, passing, with its traps proven by breaking the code.
- **Watcher replay test:** against a saved snapshot containing a known change, the Watcher must
  emit exactly one queue entry with the correct schema'd drift signal. Status: built, passing, with
  its traps proven by breaking the code. The sources read do not state what the built test asserts
  beyond that it is a regression test.
- **Every assertion is authored with its negative control run, or it is not an assertion.** An
  assertion that cannot fail is worse than none: it manufactures freshness on a schedule. The
  decision recording this found it on contact — drafting the first assertions produced one that could
  not fail, because the shell it compared never read the file in question.

Superseded 2026-10-02: the Verifier criterion first read "must produce a `contradictions.md` flag
(not a false-fresh stamp)". The decision recording the Verifier as built replaced the flag with
"reported, fact untouched"; the false-fresh-stamp half stands.

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§5A, §6 Phase 4, §9.18) · [[knowledge-agent-architecture-combination-plan]] (§5A, superseded) · [[adr-0009-recheck-assertions]] · [[adr-0012-phase-4-safety-scoped-to-one-maintainer]]
Verified: 2026-10-02 · by: agent · method: doc-review

## Orchestrator cost bounds

Status: no orchestrator is built. The plan called for a thin orchestrator that routes with capped
fan-out over per-domain workers. It assumed one worker per domain; the system built has one
retrieval agent, `kb-retrieve`, serving every library. Libraries are found by globbing
`kb/*/INDEX.md`, and each library's `INDEX.md` is its only router, so the routing an orchestrator
would do is already the first step of `kb-retrieve`'s contract: "Start at a router, never at a
guess," and "One library unless the question genuinely spans two."

The evidence, from a sample on 2026-09-28: two questions from each of the six libraries then
installed, drawn by a seeded random draw from their golden sets and including one cross-root
case, put to three `kb-retrieve` agents with four questions each.

- Right library: 12 of 12.
- Expected file: 11 of 12. On the cross-root question the agent answered correctly from both
  libraries but cited a library's router rather than the file that carries the cross-library link.
- Content read from a library the question did not need: none. Beyond the routers, each agent
  opened only the one library the answer came from, or two on the cross-root question.
- The only overhead was reading the routers: each agent read all six `INDEX.md` files once.

Limits stated by the decision: the evidence is a sample of 12 questions, not all of them (about 90);
each agent answered four questions, so its router reads were shared, and a question asked alone pays
for all six routers. That stays cheap at six libraries and is the first thing to watch as the number
grows.

The cost bound the orchestrator was specified to provide, kept from the plan:

- **Route, never broadcast.** Fan-out multiplies token cost **3–15×**; a 4-worker broadcast can
  cost **~20×** a direct lookup. Single-domain queries **pass through** to one worker; the
  orchestrator fans out **only** for confirmed cross-domain questions, **capped at 2 workers** per
  interactive query. These are the plan's figures; the sample above measured routing accuracy and
  which libraries were opened, not token cost.
- As built, the plan's cost bound is met by the retrieval contract itself, per the decision: the
  sample showed the pass-through and the two-worker cap the plan asks of an orchestrator, observed
  in the sample.
- **CI cold-start.** Clone the general repo `--depth 1` at the pinned tag with a **cache key**;
  `--sparse` to only the domains a project queries. This is the plan's text for the earlier topology
  of one general repo; content has since moved into per-domain library repositories, and no source
  read records a cold-start mechanism built.

Reopen the no-orchestrator decision when any of these holds:

- Routing accuracy falls below 95% on a golden-set sample, or a full run.
- The library count outgrows single-agent routing: reading every router stops being cheap well
  before the plan's ~15-topic sub-index threshold applies to the library set itself, and at that
  point the plan's parallel index reads and a routing layer start to pay.
- A real need for several workers appears: a question that needs specialised workers with different
  tools, or workers operating on behalf of different people, which one read-only retrieval agent
  cannot serve.

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§5A, §6 Phase 4) · [[knowledge-agent-architecture-combination-plan]] (§5A, superseded in part) · [[adr-0011-no-orchestrator-until-routing-fails]] · [[adr-0007-engine-content-separation]]
Verified: 2026-10-02 · by: agent · method: doc-review

## Cautions: human ownership and data handling

- **The verifier's logic decides truth — review it like the facts it checks, and test it** (see
  above).
- **PII handling.** Agents touching live data must not log PII to any persistent or
  developer-readable store (KB, transcripts, logs); the Verifier reduces results to the
  **minimum needed** (count/boolean, not raw rows); name a data steward for agent output.
- **A human owns each domain (CODEOWNERS). The librarian agents assist the owner; they do not
  replace them.** As built, `CODEOWNERS` is advisory (see "Mutation governance"): it names who a
  finding belongs to and does not yet enforce owner review.

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§5A) · [[adr-0002-governance-without-enforcement]] · [[script-kb-owner]]
Verified: 2026-10-02 · by: agent · method: doc-review
