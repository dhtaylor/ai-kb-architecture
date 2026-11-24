---
name: knowledge-architecture-golden
description: Golden retrieval set for the knowledge-architecture domain — routing and answer-grounding cases for design-thesis, releases-and-contract-versioning, agent-lifecycle-roles, unattended-preparation, retrieval-agent-behavior, secret-governance, guardrail-verification, retrieval-eval-integrity and the open questions.
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
of the revised plan (target layering and lifecycle roles as built) and by the secret-governance fold
(the three-pass scan, the capture boundary, and the blind trap test), and by the first general-lessons
fold, c1 (retrieval-eval-integrity, the guardrail lessons on reading versus running, a recurring defect and a
scanner that exited 0, the steady-state defaults, and the as-built Verifier note in design-thesis), and by
the second general-lessons fold, c2 (the guardrail lessons on silent skipping, topology, the contract, defaults and
tests that pass for the wrong reason), and by the engine-sync fold of 2026-10-16 (the library registry and
engine-owned hook, agent_ttl, kb-sources, archived artifacts and UTF-16 in the secret scan, and the
quoting, transcript and wrong-corpus lessons in retrieval-eval-integrity), and by the engine-sync fold of
2026-10-28 (releases-and-contract-versioning, unattended-preparation and retrieval-agent-behavior, the
unattended Verifier, the Watcher exit codes and kb-sources request sequence, the redaction filter, the
regression suite's git environment, the grader's contamination and attribution rules, and the retrieval
cost and sample tooling), and by the engine-sync fold of 2026-11-09 (contract versions 2.4 to 2.6,
ADR-0020's links_to, dependency mismatches and read-only consumers), and by fold B of the same date
(triage and specialists, the files-first search and stop rule, and the runs that found nothing), and by
fold C of the same date (current state and check-transient, sign-in redirects and manual sources, the
nudge's new-since-sweep count and archived trees, dotted names, and the check-xlinks usage error).

Cross-root case: **not applicable** — the domain links into no other library. It once linked into
claude-code-runtime, which is not published, so a reader of the published library could not follow
those links; on 2026-11-09 they became plain text, and the two records that tested the hops now test
the answer this library holds. Unresolved case: **not applicable** — nothing in the domain today carries `status: CONFLICTED`, so a genuine
UNRESOLVED case cannot yet be written without fabricating a conflict. Add one the day a real
contradiction lands.

```yaml
- case: positive
  question: What is the core design rule for combining a governed KB with an agent layer?
  expected_file: design-thesis.md
  expected_excerpt: "separate knowledge from behavior, and let the behavior layer keep the knowledge layer honest"
  expected_evidence: word

# Amended 2026-10-28, after the retrieval run and the audit: the excerpt was a two-word label, which an answer can match without having read the file. It is now the sentence that says which role splits.
- case: positive
  question: What are the three librarian roles that keep this kind of KB current, and which one splits into two?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "more than one job, split by source of truth"

- case: positive
  question: Is there an orchestrator that routes questions across libraries, and if not, what does the routing?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "no orchestrator is built"
  alt_excerpt: "the system built has one retrieval agent"

- case: positive
  question: Does rotating a leaked secret remove it from git history?
  expected_file: secret-governance.md
  expected_excerpt: "does not erase it from git"
  expected_evidence: check

- case: negative
  question: What is the production database's deploy target for this workspace?
  expected_file: ""
  expected_excerpt: "fact not found — check KB"

- case: negative
  question: What did the maintainer's personal debugging session on Tuesday conclude?
  expected_file: ""
  expected_excerpt: "fact not found — check KB"

# Amended 2026-11-09: was cross-root; the link it followed into claude-code-runtime is now plain text.
- case: positive
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
  expected_excerpt: "the engine's own CI ran only exec-bit, secret and embedded-fact checks"

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
  expected_file: unattended-preparation.md
  expected_excerpt: "Nothing runs unless the user installs it"

- case: positive
  question: Is the two-reviewer, 24-hour rule for high-blast-radius facts enforced?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "dormant until a second owner exists"

- case: positive
  question: What would make the decision not to build an orchestrator be reopened?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "Routing accuracy falls below 95%"

- case: positive
  question: Why did refusals obtained while the golden set was routed from the domain router prove nothing?
  expected_file: retrieval-eval-integrity.md
  expected_excerpt: "nobody can distinguish retrieval from recitation"

- case: positive
  question: Why did a correct, grounded answer fail the first run of the answer-grounding eval?
  expected_file: retrieval-eval-integrity.md
  expected_excerpt: "a correct, grounded answer failed on a pair of asterisks"

- case: positive
  question: How does the grader treat a cited path that names a same-named file in another library?
  expected_file: retrieval-eval-integrity.md
  expected_excerpt: "It now resolves a cited path to a file and still rejects a same-named file in another library"

# Amended 2026-10-28, after the retrieval run and the audit: the question asked about a rule the retrieval agent's own instructions state, so the agent could answer it without the library, and a Haiku run in the 2026-10-26 retest cited the contract instead of a library file. It now asks for a measured result only this library holds.
- case: positive
  question: How did the second library's retrieval eval score across its four runs?
  expected_file: retrieval-eval-integrity.md
  expected_excerpt: "went 9/16, 14/16, 11/16 and 15/16 across four runs"

- case: positive
  question: Why did an agent that handled a disputed fact correctly still fail the eval?
  expected_file: retrieval-eval-integrity.md
  expected_excerpt: "still failed because the oracle expected the literal"

# Amended 2026-10-28, after the retrieval run and the audit: the alternative excerpt stated the amendment rule, a neighboring fact, and not the reason an oracle must not follow the agent. It now states that reason.
- case: positive
  question: Why not add alternative excerpts to the oracle after each run's misses?
  expected_file: retrieval-eval-integrity.md
  expected_excerpt: "adding alternatives after each run would make the oracle follow the agent"
  alt_excerpt: "an oracle that follows the agent measures nothing"

- case: positive
  question: Why did one agent refuse four answerable questions with confidence?
  expected_file: retrieval-eval-integrity.md
  expected_excerpt: "One agent concluded that library was the only one installed and refused four answerable questions with confidence"

- case: positive
  question: What does a clean pass say about a protocol that changed between eval runs?
  expected_file: retrieval-eval-integrity.md
  expected_excerpt: "a clean pass, or anything about a protocol that changed between runs rather than one repeated"

- case: positive
  question: When independent reviewers keep reporting the same false positive on a clean control, what is checked first?
  expected_file: retrieval-eval-integrity.md
  expected_excerpt: "check the key before the reviewer"

- case: positive
  question: What does a single trial per case in these evals establish?
  expected_file: retrieval-eval-integrity.md
  expected_excerpt: "say which way things point, not how often they happen"

- case: negative
  question: How many trials per case does a retrieval eval need to measure frequency rather than direction?
  expected_file: ""
  expected_excerpt: "fact not found — check KB"

- case: positive
  question: What was found to be the highest-value technique for finding the defects in the guardrails?
  expected_file: guardrail-verification.md
  expected_excerpt: "handing an artifact to someone who had not written it and asking what was ambiguous"

- case: positive
  question: Why did the secret scan reject the contributor guide's own redaction example?
  expected_file: guardrail-verification.md
  expected_excerpt: "because that shape matches a real credential"

- case: positive
  question: What finally held after the non-executable-script defect had recurred?
  expected_file: guardrail-verification.md
  expected_excerpt: "The fix that held was not another fix but a check"

# Amended 2026-10-28, after the retrieval run and the audit: the alternative excerpt said why such a scanner is worse than none, a neighboring fact, and not why it exited 0. It now states the cause in the scanner header's own words.
- case: positive
  question: Why did a secret scanner print its findings and still exit 0?
  expected_file: guardrail-verification.md
  expected_excerpt: "so the counter it incremented lived in a subshell and was discarded"
  alt_excerpt: "a pipeline component runs in a subshell, so a counter incremented there is discarded and the script would print findings while exiting 0"

- case: positive
  question: Why would the first draft of the session-start nudge have been ignored?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "so it would have asked about them at every session start, forever"

- case: positive
  question: What did the first draft of the session-start hook cost, and why?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "took the hook from 0.06s to 3s"

# Amended 2026-10-28, after the retrieval run and the audit: design-thesis held a second statement of the Verifier as built, and the 2026-10-26 routing retest failed this record because the agent answered from the home file and never read design-thesis. The owner decided to cut the copy to a pointer, so the record points at the home file and quotes its wording.
- case: positive
  question: As built, does the Verifier query live systems to re-verify a fact?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "and it does not query live systems"
  alt_excerpt: "an assertion, not a query, where exit 0 means the fact still holds"

- case: positive
  question: Which passes does check-secrets run, and does a hit in any one of them block?
  expected_file: secret-governance.md
  expected_excerpt: "runs three passes per file, and any pass blocks"

- case: positive
  question: Can the secret scan detect a person's name written in prose?
  expected_file: secret-governance.md
  expected_excerpt: "it cannot detect a personal NAME in prose"

- case: positive
  question: How does a line holding a deliberate example get past all of the secret scan's passes?
  expected_file: secret-governance.md
  expected_excerpt: "to clear a legitimate match from all three"

- case: positive
  question: Why does kb-capture refuse a note holding a credential instead of leaving the check to the commit hook?
  expected_file: secret-governance.md
  expected_excerpt: "Retrieval reads the working tree before any commit hook runs"

- case: positive
  question: Where does a note go when kb-capture refuses it?
  expected_file: secret-governance.md
  expected_excerpt: "outside the tree, with the retry command printed"

- case: positive
  question: Does the worktree secret scan cover files that are not yet tracked by git?
  expected_file: secret-governance.md
  expected_excerpt: "scans untracked, non-ignored files"

- case: positive
  question: Where is the rule for recording a security finding stated, and is it repeated in this library?
  expected_file: contract-provenance-and-currency.md
  expected_excerpt: "never the literal value"

- case: positive
  question: What did the blind trap test of the distill gate record in place of a pasted password?
  expected_file: secret-governance.md
  expected_excerpt: "It recorded the finding by vault reference, commit and rotate action"

- case: negative
  question: Is push protection switched on for the library's repository?
  expected_file: ""
  expected_excerpt: "fact not found — check KB"

- case: positive
  question: What happened to the project hook's secret scan when a structure check chained before it failed?
  expected_file: guardrail-verification.md
  expected_excerpt: "any structural failure skipped the secret scan, so a pasted secret surfaced only on the retry"

- case: positive
  question: Why did a pre-commit hook fired outside an interactive terminal skip its checks while the commit went through?
  expected_file: guardrail-verification.md
  expected_excerpt: "found no engine and skipped its checks while exiting 0"

# Amended 2026-10-28, after the retrieval run and the audit: the record used the older cross-root form, which names a file in this library and checks only where the hop starts, and the 2026-10-26 retest found agents going straight to the other library as the question invites. It now names the answering file in the other library, as contract section 10 specifies, and asks for what that file states.
# Amended 2026-11-09: was cross-root into claude-code-runtime:config-resolution.md; that link is now plain text, and guardrail-verification states the same excerpt.
- case: positive
  question: "The engine's pre-commit hook found no engine and skipped its checks when fired outside an interactive terminal. What second channel did the fix give hooks, and why did the engine path exported from the shell startup file not reach that hook?"
  expected_file: guardrail-verification.md
  expected_excerpt: "returns early for non-interactive shells"

- case: positive
  question: Has a GUI git client or IDE been tested against the engine hook?
  expected_file: guardrail-verification.md
  expected_excerpt: "a GUI git client or IDE has never been tested against a hook, only reasoned about"

- case: positive
  question: Why did restructuring the repositories invalidate the guardrail testing done before it?
  expected_file: guardrail-verification.md
  expected_excerpt: "survives a change to that topology no better than a hardcoded path does"

- case: positive
  question: Why record a rule the architecture made unnecessary as obsolete instead of deleting it?
  expected_file: guardrail-verification.md
  expected_excerpt: "a rule quietly dropped and a rule deliberately retired are indistinguishable"

- case: positive
  question: Why is a contract that contradicts itself a worse failure than one that disagrees with a skill?
  expected_file: guardrail-verification.md
  expected_excerpt: "the precedence rule has nothing to say"

# Amended 2026-10-28, after the retrieval run and the audit: the file no longer says "now" about the default, so the excerpt drops that word.
- case: positive
  question: Does kb-bootstrap change a machine's configuration when run without an apply flag?
  expected_file: guardrail-verification.md
  expected_excerpt: "reports and changes nothing unless given"

- case: positive
  question: Why would a regression case that typed a literal date have failed by itself?
  expected_file: guardrail-verification.md
  expected_excerpt: "would have started failing on its own once the sweep interval elapsed"

- case: positive
  question: Why did the end-to-end check for a drift record pass when no drift had been recorded?
  expected_file: guardrail-verification.md
  expected_excerpt: "because the word appears in the queue's own keywords line"

- case: positive
  question: Is a CI job that fails after a push the enforcement boundary for secrets?
  expected_file: secret-governance.md
  expected_excerpt: "after a push has landed is detection behind that boundary"

- case: positive
  question: Why does the per-machine library registry live outside every repository?
  expected_file: design-thesis.md
  expected_excerpt: "describes the machine it is on, so it cannot lie about another one"

- case: positive
  question: Why were libraries no longer discovered by being present in the engine's kb folder?
  expected_file: design-thesis.md
  expected_excerpt: "which looks exactly like a correct refusal"

- case: positive
  question: Why does a domain library no longer carry its own pre-commit hook?
  expected_file: design-thesis.md
  expected_excerpt: "moving it or upgrading the engine can never require a commit to it"

- case: positive
  question: Why may a domain declare its own agent TTL instead of a tenth of its horizon?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "so every agent-verified section was stale at every sweep"

- case: positive
  question: Does kb-sources fetch a source URL without the operator's approval?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "NOTHING IS FETCHED WITHOUT --yes"

- case: positive
  question: Which secret checks still run on a raw artifact archived under a library's documents folder?
  expected_file: secret-governance.md
  expected_excerpt: "The patterns, the labelled rule and PII still run on every line of it."

- case: positive
  question: Why does check-secrets block a UTF-16 text file?
  expected_file: secret-governance.md
  expected_excerpt: "reads as binary to every pass and so would be skipped unseen"

- case: positive
  question: Why did correct, cited answers fail grade-eval before the agent was told to quote?
  expected_file: retrieval-eval-integrity.md
  expected_excerpt: "correct, grounded, cited answers failed whenever they paraphrased"

# Amended 2026-10-28, after the retrieval run and the audit: the question asked about a rule the retrieval agent's own instructions state, and a Haiku run in the 2026-10-26 retest cited the contract instead of a library file. It now asks for a measured result only this library holds.
- case: positive
  question: On its first grading, how did the first routing run graded from tool calls score, and what caused its failures?
  expected_file: retrieval-eval-integrity.md
  expected_excerpt: "scored 6/12 at first grading, and all six failures were one grader bug"

# Boundary cases: owner-approved 2026-10-26

- case: positive
  question: "A pre-commit hook for the knowledge base ran from a non-interactive shell and exited 0 without running any checks, because the engine path was exported in .bashrc. What fix did the architecture adopt?"
  expected_file: "guardrail-verification.md"
  expected_excerpt: "The decision's fix gives hooks a second channel, git's global configuration"

- case: positive
  question: "If plugin behaviour is loaded once at session start but facts are read live on each query, does the design principle of separating knowledge from behaviour still hold?"
  expected_file: "design-thesis.md"
  expected_excerpt: "The separation holds; the two halves simply refresh on different clocks."

- case: positive
  question: "Since a git pre-commit hook can be skipped with --no-verify, where does the architecture say secret enforcement actually has to live?"
  expected_file: "secret-governance.md"
  expected_excerpt: "a pre-commit hook is bypassable with --no-verify, so the enforcement boundary is the server-side gate"

# Engine-sync fold 2026-10-28

- case: positive
  question: "Why was the engine's first release numbered 0.2.0 and not 0.1.0?"
  expected_file: "releases-and-contract-versioning.md"
  expected_excerpt: "Tagging a release with that number would have given two different things one name."

- case: positive
  question: "What may be merged to main during the two days before a release?"
  expected_file: "releases-and-contract-versioning.md"
  expected_excerpt: "No code, check or contract change merges in that window."

- case: positive
  question: "If the release work finds a defect during the freeze, who decides what happens?"
  expected_file: "releases-and-contract-versioning.md"
  expected_excerpt: "A defect the release work finds in the window is the owner's call"

- case: positive
  question: "Does an engine release move the version a library's CI pins?"
  expected_file: "releases-and-contract-versioning.md"
  expected_excerpt: "an engine release moves no library's pin"

- case: positive
  question: "Does a library valid under an earlier minor of the contract stay valid when a new minor ships?"
  expected_file: "releases-and-contract-versioning.md"
  expected_excerpt: "A library valid under an earlier minor of the same major stays valid and stays served."

- case: positive
  question: "What contract version is a library read as when its root index declares none?"
  expected_file: "releases-and-contract-versioning.md"
  expected_excerpt: "A library that declares nothing is read as 2.0"

- case: positive
  question: "A library declares contract 2.0 and the engine adds a new rejection in 2.1. Does that library's check fail?"
  expected_file: "releases-and-contract-versioning.md"
  expected_excerpt: "gets a warning that names the rule and the version that brings it in, and its check still passes"

- case: positive
  question: "Why does the engine refuse to serve a library on an unsupported contract major instead of serving it with a warning?"
  expected_file: "releases-and-contract-versioning.md"
  expected_excerpt: "its markers may mean something this engine does not read"

- case: positive
  question: "How many contract majors does an engine support?"
  expected_file: "releases-and-contract-versioning.md"
  expected_excerpt: "An engine supports every minor of the current major and of the one before it"

- case: positive
  question: "Is migrating a library to contract 2.3 required?"
  expected_file: "releases-and-contract-versioning.md"
  expected_excerpt: "Nothing is required to migrate."

- case: positive
  question: "In a scheduled, unattended recheck, what happens to an unmarked assertion that needs a credential?"
  expected_file: "agent-lifecycle-roles.md"
  expected_excerpt: "unmarked assertion that needs one fails rather than reaching it"

- case: negative
  question: "When is the engine's next release?"
  expected_file: ""
  expected_excerpt: "fact not found — check KB"

- case: negative
  question: "Which contract version will the engine publish next?"
  expected_file: ""
  expected_excerpt: "fact not found — check KB"

# Amended 2026-10-28, after the retrieval run and the audit: the record duplicated the record above it, same file and same excerpt. It now tests when the opt-in schedule runs.
- case: positive
  question: "When does the opt-in schedule run, and what happens if the machine was off at that time?"
  expected_file: "unattended-preparation.md"
  expected_excerpt: "a systemd user timer on Monday at 07:00 that catches up a missed week at the next start"

- case: positive
  question: "Why can the headless preparation run not change a library?"
  expected_file: "unattended-preparation.md"
  expected_excerpt: "It has no write, shell or network tool, so it cannot change a library or send what it reads anywhere."

- case: positive
  question: "Do kb-update-domain and kb-organize-domain have an unattended preparation step?"
  expected_file: "unattended-preparation.md"
  expected_excerpt: "and both stay fully attended"

- case: positive
  question: "Can an unattended preparation run produce a wrong review packet even though it cannot write?"
  expected_file: "unattended-preparation.md"
  expected_excerpt: "bounds what the model can do, not what it concludes"

- case: positive
  question: "Does the weekly report hold the text of any fact, source or packet?"
  expected_file: "unattended-preparation.md"
  expected_excerpt: "a fact's, a source's or a packet's text: paths, counts, headings and ids only"

- case: positive
  question: "Why does the weekly report write a file even when nothing needs a decision?"
  expected_file: "unattended-preparation.md"
  expected_excerpt: "so a quiet week can be told from a week the job did not run"

- case: positive
  question: "What motivated the rule that a scheduled command must work from any directory?"
  expected_file: "unattended-preparation.md"
  expected_excerpt: "reported FAIL for every backup for a week"

- case: positive
  question: "Which actions stay attended whatever the schedule does?"
  expected_file: "unattended-preparation.md"
  expected_excerpt: "any fact edit without a person's approval are never automated"

- case: positive
  question: "Why is a backup bundle verified by restoring it and not by git's own bundle check?"
  expected_file: "unattended-preparation.md"
  expected_excerpt: "so a truncated bundle passes it"

- case: negative
  question: "What does the scheduled weekly preparation cost per month across all libraries?"
  expected_file: ""
  expected_excerpt: "fact not found — check KB"

- case: positive
  question: "What does kb-watch exit with when a source drifted or was unreachable?"
  expected_file: "agent-lifecycle-roles.md"
  expected_excerpt: "It exits 0 when no source drifted or was unreachable, 1 when any did, and 2 on a configuration error"

- case: positive
  question: "What does kb-sources do when a server answers 403 to its own User-Agent?"
  expected_file: "agent-lifecycle-roles.md"
  expected_excerpt: "A 403 to the tool's own User-Agent is retried once with Python's default User-Agent"

- case: positive
  question: "When does kb-sources class a source as dead?"
  expected_file: "agent-lifecycle-roles.md"
  expected_excerpt: "only when the GET answers 404 or 410"

- case: positive
  question: "Why did the session-start nudge not reach the person at first?"
  expected_file: "agent-lifecycle-roles.md"
  expected_excerpt: "which reaches the model and is not shown to the person"

- case: positive
  question: "Why does the nudge report stale facts with no assertion as their own category?"
  expected_file: "agent-lifecycle-roles.md"
  expected_excerpt: "the nudge printed nothing while 49 agent-stamped facts were past their TTL"

- case: positive
  question: "After the rule on what the knowledge base is changed, how was it re-tested, and what does that re-test show?"
  expected_file: "retrieval-agent-behavior.md"
  expected_excerpt: "three trials of one question show the rule is followed for the question that failed"

# Amended 2026-10-28, after the retrieval run and the audit: the question asked about a procedure the retrieval agent's own instructions state, so the agent answered it without the library. It now asks for a threshold only this library holds.
- case: positive
  question: "How much must two libraries' keywords and tags overlap before registering the second one warns?"
  expected_file: "retrieval-agent-behavior.md"
  expected_excerpt: "warns when two libraries share at least 3 terms and those terms are at least a quarter of the smaller library's terms"

# Amended 2026-10-28, after the retrieval run and the audit: the question asked about a rule the retrieval agent's own instructions state. It now asks for what the sample taken after section reads began found about the disputed case.
- case: positive
  question: "What did the sample taken after section reads began find about the disputed case?"
  expected_file: "retrieval-agent-behavior.md"
  expected_excerpt: "a disputed section still refused, because the section read held its CONFLICTED marker"

# Amended 2026-10-28, after the retrieval run and the audit: the question asked about a rule the retrieval agent's own instructions state. It now asks for a measured result only this library holds.
- case: positive
  question: "In the section-read sample, two agents searched across the libraries' parent folder before reading a router. What did those searches match?"
  expected_file: "retrieval-agent-behavior.md"
  expected_excerpt: "matched only the one library each needed, so neither case failed on it"

# Amended 2026-10-28, after the retrieval run and the audit: the question asked about a rule the retrieval agent's own instructions state. It now asks why the overlap test at registration needs a minimum count, a design reason only this library holds.
- case: positive
  question: "Why does the overlap test at registration require at least 3 shared terms?"
  expected_file: "retrieval-agent-behavior.md"
  expected_excerpt: "the count of 3 stops two short lists matching on one word"

# Amended 2026-10-28, after the retrieval run and the audit: the record had already been reworded once after the retrieval run, and it still asked about a rule the retrieval agent's own instructions state. It now asks why a retrieval subagent's session context leaves out the due-nudge, a design reason only this library holds.
- case: positive
  question: "Why does the session context given to a retrieval subagent leave out the librarian's due-nudge?"
  expected_file: "retrieval-agent-behavior.md"
  expected_excerpt: "The librarian's due-nudge is left out, because it is for the person's session and not for a retrieval."

- case: positive
  question: "Does the overlap warning when a library is added ever refuse the registration?"
  expected_file: "retrieval-agent-behavior.md"
  expected_excerpt: "It never changes the exit status and writes nothing."

- case: positive
  question: "Why does the overlap test measure against the smaller library's terms and not use Jaccard?"
  expected_file: "retrieval-agent-behavior.md"
  expected_excerpt: "because Jaccard hides a small library sitting inside a big neighbor's vocabulary"

- case: positive
  question: "Why does a retrieval subagent need a hook to be given the engine path?"
  expected_file: "retrieval-agent-behavior.md"
  expected_excerpt: "A subagent never sees the session-start context"

- case: positive
  question: "Which kind of registered library does the subagent hook give as an example of one that is not served?"
  expected_file: "retrieval-agent-behavior.md"
  expected_excerpt: "A library on an unsupported contract major is one such case"

# Amended 2026-10-28, after the retrieval run and the audit: the file no longer says the agent was right about the record, because the eval record does not say so. The record asks why the two graders disagreed, which the eval record does state.
- case: positive
  question: "An agent answered a question the library cannot answer by reading an engine decision record. Why did grade-eval fail the case while the routing grader passed it?"
  expected_file: "retrieval-eval-integrity.md"
  expected_excerpt: "because that grader did not count a read outside every registered library"

- case: positive
  question: "Could the first sample taken after section reads began measure the token saving?"
  expected_file: "retrieval-agent-behavior.md"
  expected_excerpt: "That sample could not measure the token saving"

- case: negative
  question: "How many tokens does reading a section instead of a whole file save per question?"
  expected_file: ""
  expected_excerpt: "fact not found — check KB"

- case: positive
  question: "Does lint-allow exempt a line from the redaction filter?"
  expected_file: "secret-governance.md"
  expected_excerpt: "because in text being filtered it is just more input"

- case: positive
  question: "What does the redaction filter do when it cannot scan its input?"
  expected_file: "secret-governance.md"
  expected_excerpt: "so a caller holds the input rather than passing it on unscanned"

- case: positive
  question: "Does the redaction filter print the text that matched?"
  expected_file: "secret-governance.md"
  expected_excerpt: "the matched text appears nowhere"

- case: positive
  question: "Why did the regression suite change a shared repository's configuration when run from a commit in a linked worktree?"
  expected_file: "guardrail-verification.md"
  expected_excerpt: "then act on the repository being committed to and not on their throwaway fixtures"

- case: positive
  question: "How was the regression suite's inherited git environment repaired?"
  expected_file: "guardrail-verification.md"
  expected_excerpt: "so each fixture runs against its own repository"

- case: positive
  question: "Why did nothing show on the machine that wrote the suite when its fixtures aimed at the wrong repository?"
  expected_file: "guardrail-verification.md"
  expected_excerpt: "only a commit from a linked worktree, through the hook, carried them in"

- case: positive
  question: "What counts as contamination for the routing grader?"
  expected_file: "retrieval-eval-integrity.md"
  expected_excerpt: "Contamination is what the retrieval contract forbids"

- case: positive
  question: "Why could the routing grader not tell which library content came from when a Grep targeted a single file?"
  expected_file: "retrieval-eval-integrity.md"
  expected_excerpt: "ripgrep omits the file name from content lines"

- case: positive
  question: "Why could a golden excerpt written in single quotes never match?"
  expected_file: "retrieval-eval-integrity.md"
  expected_excerpt: "was searched for with its quotes still on and could never match"

- case: positive
  question: "Who should write boundary cases for a golden set?"
  expected_file: "retrieval-eval-integrity.md"
  expected_excerpt: "written by someone who has not seen the routers' wording"

- case: positive
  question: "Why does kb-telemetry count each message id once?"
  expected_file: "retrieval-eval-integrity.md"
  expected_excerpt: "One API message is logged in several records, so each message id is counted once."

- case: positive
  question: "Why does kb-telemetry leave cache reads out of new tokens?"
  expected_file: "retrieval-eval-integrity.md"
  expected_excerpt: "they are the cheap re-reading of what is already cached, not new work"

- case: positive
  question: "Why does kb-telemetry start its clock at the first API response and not at the spawn?"
  expected_file: "retrieval-eval-integrity.md"
  expected_excerpt: "because its queue time would dominate"

- case: positive
  question: "Why does kb-eval-sample accept an explicit list of cases?"
  expected_file: "retrieval-eval-integrity.md"
  expected_excerpt: "A random sample can miss what a reshape changed"

# Amended 2026-10-28, after the retrieval run and the audit: the excerpt carried backticks, which contract section 10 forbids in an excerpt, and the not-served rationale beside it was a second copy of the one in releases-and-contract-versioning, now cut to a pointer. The record asks for the fifth state, which design-thesis defines in plain text.
- case: positive
  question: "A registered library can be ok, missing, not a library or a name mismatch. What is the fifth state?"
  expected_file: "design-thesis.md"
  expected_excerpt: "a library whose declared contract version names a major the engine does not support, or cannot be read"

- case: positive
  question: "What ties a library to the engine's rules, if not a pin on the engine?"
  expected_file: "releases-and-contract-versioning.md"
  expected_excerpt: "Rules apply by the declared version."

# Amended 2026-11-09, engine-sync fold B: triage and specialists are now built (ADR-0018, Proposed), and the open question this record pointed at was replaced. The question is unchanged; the answer moved to the file that now states it.
- case: positive
  question: "Is retrieval as triage plus specialists built?"
  expected_file: "retrieval-agent-behavior.md"
  expected_excerpt: "The default way to answer from the knowledge base is the `kb-ask` skill, a triage desk that runs in the main session"

- case: positive
  question: "Does the knowledge-architecture library get its own version number, or follow the engine's?"
  expected_file: "releases-and-contract-versioning.md"
  expected_excerpt: "on the same day, at the library commit verified against that engine"

- case: positive
  question: "Why is there no knowledge-architecture library v0.3.0?"
  expected_file: "releases-and-contract-versioning.md"
  expected_excerpt: "so this library goes from v0.2.0 to v0.4.0, and no library v0.3.0 exists"

- case: positive
  question: "When does a fix to the reference library alone get released?"
  expected_file: "releases-and-contract-versioning.md"
  expected_excerpt: "but it gets no tag of its own"

- case: positive
  question: Why does a freshly cloned repository not run the commit checks until I run kb-init in it?
  expected_file: using-ai-kb.md
  expected_excerpt: "local config that git does not clone"

- case: positive
  question: What does running kb-bootstrap with no flag do on a machine?
  expected_file: using-ai-kb.md
  expected_excerpt: "reports drift, changes nothing, exits 1 if anything is out of step"

- case: positive
  question: What is the command to make session start show the nudge only on Mondays?
  expected_file: using-ai-kb.md
  expected_excerpt: "kb.nudge weekly:mon"

- case: positive
  question: What does a commit hook say when this machine has no engine registered?
  expected_file: using-ai-kb.md
  expected_excerpt: "engine not found — structural and secret checks skipped"

- case: positive
  question: Why does ai-kb keep the knowledge root in user-level settings and not in a project file?
  expected_file: using-ai-kb.md
  expected_excerpt: "A project file can be overwritten by the repo it lives in"

- case: positive
  question: How do I move my registered libraries to a second machine?
  expected_file: using-ai-kb.md
  expected_excerpt: "kb-library export > libraries.txt"

- case: positive
  question: What does the engine's own pre-commit hook need configured in each clone, and how long does the test suite take?
  expected_file: using-ai-kb.md
  expected_excerpt: "takes about a minute and must stay green"

- case: negative
  question: How do I install ai-kb through a package manager such as apt or npm?
  expected_file: ""
  expected_excerpt: "fact not found — check KB"

- case: negative
  question: Which ai-kb command pushes a library to its remote automatically after each capture?
  expected_file: ""
  expected_excerpt: "fact not found — check KB"

- case: positive
  question: Why does ai-kb register its plugin explicitly in a consumer's settings instead of relying on directory nesting?
  expected_file: skills-agents-and-commands.md
  expected_excerpt: Explicit registration is the feature.
- case: positive
  question: After editing a skill in the plugin, what should I do before trusting it?
  expected_file: skills-agents-and-commands.md
  expected_excerpt: edit a skill, restart before trusting it.
- case: positive
  question: Does a skill's allowed-tools list stop it from using other tools?
  expected_file: skills-agents-and-commands.md
  expected_excerpt: only pre-approves the named tools for the turn that invokes it
- case: positive
  question: When does kb-update-domain refuse a fold because of its source?
  expected_file: skills-agents-and-commands.md
  expected_excerpt: the source material is not actually available to you.
- case: positive
  question: What does kb-organize-domain do when making the structure nicer would change what a fact means?
  expected_file: skills-agents-and-commands.md
  expected_excerpt: you would have to change a fact's meaning to make the structure nicer.
- case: positive
  question: Is kb-audit allowed to fix the problems it finds?
  expected_file: skills-agents-and-commands.md
  expected_excerpt: Do NOT use it to fix what it finds.
- case: positive
  question: How does the /kb-capture command treat free text typed after the command?
  expected_file: skills-agents-and-commands.md
  expected_excerpt: a hint for the title, not an argument
- case: positive
  question: What must the /kb-init command do when it cannot find the engine, and may it build the scaffold itself?
  expected_file: skills-agents-and-commands.md
  expected_excerpt: Do not reconstruct the scaffold by hand
- case: positive
  question: In intelligence-analysis coach mode, what happens to the turn after the skill asks a question?
  expected_file: skills-agents-and-commands.md
  expected_excerpt: that question ends your turn.
- case: negative
  question: Which ai-kb skill translates a library's facts into another language?
  expected_file:
  expected_excerpt: fact not found — check KB
- case: negative
  question: How many tokens does one /kb-init run cost?
  expected_file:
  expected_excerpt: fact not found — check KB

- case: positive
  question: Is the check that refused a commit stamping by: human under an agent co-authorship trailer still in the engine?
  expected_file: script-reference-checks.md
  expected_excerpt: "is retired after v0.5.0 (#240, ADR-0022)"

- case: positive
  question: Does check-embedded-facts catch an inlined table name or enum value?
  expected_file: script-reference-checks.md
  expected_excerpt: "identifier-level facts — a table name, a column, an enum value — are not reliably machine-detectable and are NOT caught here."

- case: positive
  question: Does a skill's allowed-tools frontmatter restrict which tools it can call, according to check-tool-scope?
  expected_file: script-reference-checks.md
  expected_excerpt: "is a declaration of intent, not a fence"

- case: positive
  question: What are check-xlinks' exit codes, and does a warning fail it?
  expected_file: script-reference-checks.md
  expected_excerpt: "Exit 0 when nothing is dead, 1 otherwise, 2 on a usage error. Warnings alone do not fail."

- case: positive
  question: What rule does check-exec-bits apply to decide a tracked file must be executable?
  expected_file: script-reference-checks.md
  expected_excerpt: "anything tracked that starts with a shebang must be recorded executable, wherever it lives"

- case: positive
  question: What is the exit code of grade-routing when a case fails?
  expected_file: script-reference-checks.md
  expected_excerpt: "Exit 0 if every graded case passes, 1 otherwise, 2 on a usage or format error."

- case: positive
  question: How does kb-owner choose between two matching CODEOWNERS patterns?
  expected_file: script-reference-tools.md
  expected_excerpt: "the LAST matching pattern wins, not the most specific."

- case: positive
  question: When does kb-catalog's output go stale?
  expected_file: script-reference-tools.md
  expected_excerpt: "the catalog is built when it is asked for, so it cannot go stale."

- case: positive
  question: What are kb-backup's exit codes?
  expected_file: script-reference-tools.md
  expected_excerpt: "Exit 0 all ok, 1 any FAIL, 2 could not run."

- case: positive
  question: What do kb-eval-sample's seed and sample size default to?
  expected_file: script-reference-tools.md
  expected_excerpt: "--seed today as YYYYMMDD, --n 5."

- case: positive
  question: What exit codes does kb-schedule status give?
  expected_file: script-reference-tools.md
  expected_excerpt: "status exits 0 installed, 1 not installed."

- case: positive
  question: Why is kb-init safe to run a second time?
  expected_file: script-reference-tools.md
  expected_excerpt: "a scaffolder that is destructive on its second run is one nobody runs twice."

- case: positive
  question: What are kb-verify's exit codes?
  expected_file: script-reference-tools.md
  expected_excerpt: "Exit 0 when nothing failed, 1 when an assertion failed, 2 on usage error."

- case: positive
  question: What is the kbengine package for, and does a script configure a path to find it?
  expected_file: script-reference-tools.md
  expected_excerpt: "a version means one thing in all of them"

- case: negative
  question: What flag does kb-backup take to upload its bundles to cloud storage?
  expected_file: ""
  expected_excerpt: "fact not found — check KB"

- case: negative
  question: Which kb-owner option rewrites CODEOWNERS to assign a new owner?
  expected_file: ""
  expected_excerpt: "fact not found — check KB"

- case: positive
  question: How do I decide whether a fact is scoped product:<vendor> or general?
  expected_file: contract-layout-and-naming.md
  expected_excerpt: Would this fact stop being true if you swapped the vendor or product?

- case: positive
  question: What makes a category valid for a comparative product library?
  expected_file: contract-layout-and-naming.md
  expected_excerpt: A category is the alternatives a reader would compare for one job.

- case: positive
  question: Why is a repo-scoped fact filed in a general-tier library a problem?
  expected_file: contract-layout-and-naming.md
  expected_excerpt: is a claim about every deployment inferred from one

- case: positive
  question: May a fact file share its name with a folder in the library?
  expected_file: contract-layout-and-naming.md
  expected_excerpt: A folder name is still reserved: no file may share it, so a slug never reads as a folder

- case: positive
  question: Why does the contract treat a link into an uninstalled library differently from a dead slug?
  expected_file: contract-layout-and-naming.md
  expected_excerpt: Collapsing the two would punish portability

- case: positive
  question: Why did ADR-0010 reject a credentialed integration CI job for checking cross-library links?
  expected_file: contract-layout-and-naming.md
  expected_excerpt: A job that resolves cross-library links needs exactly the credential that design exists to avoid.

- case: positive
  question: In ADR-0003, what keeps a nested project and the general tier separate version-control roots?
  expected_file: contract-layout-and-naming.md
  expected_excerpt: Neither can commit the other's files; neither appears in the other's history.

- case: positive
  question: What is the status of the CODEOWNER scope audit in ADR-0004?
  expected_file: contract-layout-and-naming.md
  expected_excerpt: Row 7 (the CODEOWNER audit) stays dormant while there is one owner.

- case: positive
  question: Why should a small domain stay flat instead of using sub-folders?
  expected_file: contract-layout-and-naming.md
  expected_excerpt: every sub-folder adds a routing hop

- case: positive
  question: How should anything cite the contract or an INDEX.md router?
  expected_file: contract-layout-and-naming.md
  expected_excerpt: cite the contract by path, or cite the specific fact that restates its rule.

- case: positive
  question: What should the description line of a fact file say?
  expected_file: contract-layout-and-naming.md
  expected_excerpt: description: <one line — what this file answers, disambiguating from siblings>

- case: positive
  question: How long does a root INDEX.md say a kb-audit sweep stays good for?
  expected_file: contract-layout-and-naming.md
  expected_excerpt: how long a kb-audit sweep stays good for. 30d for a library, 14d for

- case: positive
  question: What does a currency stamp look like?
  expected_file: contract-provenance-and-currency.md
  expected_excerpt: Verified: 2026-09-19 · by: agent · method: query

- case: positive
  question: What does the integration-test method of a currency stamp mean?
  expected_file: contract-provenance-and-currency.md
  expected_excerpt: a real transaction against staging — the only thing that catches a vendor changing runtime behavior without touching its docs

- case: positive
  question: When is a file verified floor unknown even though other sections are freshly stamped?
  expected_file: contract-provenance-and-currency.md
  expected_excerpt: the oldest stamp among the stamped sections says nothing about the one nobody checked

- case: positive
  question: What should I do when a disputed claim shares a section with sound facts?
  expected_file: contract-provenance-and-currency.md
  expected_excerpt: Collateral refusal is a bug, not an abundance of caution

- case: positive
  question: Does a status marker under a sub-heading withhold the enclosing section too?
  expected_file: contract-provenance-and-currency.md
  expected_excerpt: never to the headings enclosing it

- case: positive
  question: How does the contract tell an uncorroborated claim apart from a hedge?
  expected_file: contract-provenance-and-currency.md
  expected_excerpt: where the weakness is the source's authority, not the reasoning.

- case: positive
  question: What is the test for a high blast radius fact?
  expected_file: contract-provenance-and-currency.md
  expected_excerpt: The test is the consequence, not the subject matter

- case: positive
  question: Why must a source artifact containing a live credential be redacted before archiving?
  expected_file: contract-provenance-and-currency.md
  expected_excerpt: the archive is a copy, and a copy leaks

- case: positive
  question: Who may mark the dependants of a dead source as provenance BROKEN, and what does the sweep do with them?
  expected_file: contract-provenance-and-currency.md
  expected_excerpt: A person triaging a dead source may write

- case: positive
  question: Can another KB file be the evidence for promoting a fact to the general tier?
  expected_file: contract-provenance-and-currency.md
  expected_excerpt: One observation is not a general truth.

- case: positive
  question: What happens to the origin file when a fact is promoted to the general tier?
  expected_file: contract-provenance-and-currency.md
  expected_excerpt: that cannot answer queries: agents follow the link, never serve stub content

- case: positive
  question: How must a Recheck assertion name an engine script?
  expected_file: contract-provenance-and-currency.md
  expected_excerpt: never by a relative path out of the library

- case: positive
  question: Why can one fetched page line not defend twenty sections of a library?
  expected_file: contract-provenance-and-currency.md
  expected_excerpt: a page cannot state twenty different specific facts in one sentence

- case: positive
  question: How may an artifact in another repository be cited?
  expected_file: contract-provenance-and-currency.md
  expected_excerpt: Never a relative path across a repository boundary

- case: positive
  question: What case types can a golden record carry?
  expected_file: contract-retrieval-and-verification.md
  expected_excerpt: case: positive | negative | unresolved | cross-root | uncorroborated

- case: positive
  question: Why must every domain carry at least two negative golden cases?
  expected_file: contract-retrieval-and-verification.md
  expected_excerpt: a knowledge base that never refuses is one that guesses.

- case: positive
  question: What does check-golden verify for an unresolved case?
  expected_file: contract-retrieval-and-verification.md
  expected_excerpt: No fact file contains that phrase

- case: positive
  question: When is a cross-root golden case required?
  expected_file: contract-retrieval-and-verification.md
  expected_excerpt: cross-root case only where the domain actually links across libraries

- case: positive
  question: How long should a golden-set excerpt be?
  expected_file: contract-retrieval-and-verification.md
  expected_excerpt: An excerpt is the one sentence, or the one clause, that carries the value

- case: positive
  question: Why may a golden excerpt not carry markdown emphasis?
  expected_file: contract-retrieval-and-verification.md
  expected_excerpt: emphasis exists in the file and never in a spoken answer

- case: positive
  question: How many checks does the contract list for verifying a domain, and are they all built?
  expected_file: contract-retrieval-and-verification.md
  expected_excerpt: All twenty-seven are built.

- case: positive
  question: What does the stamp honesty check refuse?
  expected_file: contract-retrieval-and-verification.md
  expected_excerpt: a commit claiming agent co-authorship does not also add a

- case: positive
  question: What does the high-blast gate do with a diff that deletes a knowledge file?
  expected_file: contract-retrieval-and-verification.md
  expected_excerpt: is flagged, for CI to hold until an owner labels it; detection only

- case: negative
  question: Which frontmatter field in a fact file records the person who owns or reviews it?
  expected_file: 
  expected_excerpt: fact not found — check KB

- case: negative
  question: What is the maximum length in characters of a fact file description line?
  expected_file: 
  expected_excerpt: fact not found — check KB

- case: negative
  question: What retention period does the contract set for episodic notes?
  expected_file: 
  expected_excerpt: fact not found — check KB
```
# Added 2026-11-09, engine-sync fold: contract 2.4 to 2.6 and ADR-0020.
- case: positive
  question: From which contract version does check-kb fail a section in a fact file that has no Source line, and which sections are exempt?
  expected_file: releases-and-contract-versioning.md
  expected_excerpt: "The intro under the `#` title, a heading with only subheadings under it, and a `CONFLICTED` section are exempt"

- case: positive
  question: My library links into another library. What must its root INDEX.md list under links_to, and does a link inside an inline code span need an entry?
  expected_file: releases-and-contract-versioning.md
  expected_excerpt: "A link inside an inline code span explains the form and needs no entry"

- case: positive
  question: A library I installed links into a library I already have under the same name, but mine comes from a different repository. Will check-xlinks fail my commit?
  expected_file: releases-and-contract-versioning.md
  expected_excerpt: "A mismatch warns because the person who sees it can't fix it"

- case: positive
  question: Can I commit a correction into a library I registered with kb-library add --read-only?
  expected_file: releases-and-contract-versioning.md
  expected_excerpt: "The engine's library hook refuses commits in it"

# Amended 2026-11-11: --ref pinning is built and ships in v0.4.0; the record asked whether it was built "today" and expected "Pinning is decided but not built", which is no longer true.
- case: positive
  question: Can a consumer pin a read-only library to a release tag with kb-library add --ref?
  expected_file: releases-and-contract-versioning.md
  expected_excerpt: "clones at the tag and records it as `ref` in the registry"

# Added 2026-11-09, engine-sync fold B: triage and specialists, files-first search, the stop rule, not-found runs.
- case: positive
  question: What does a kb-retrieve specialist answer when the question's topic is outside its library, and how does that differ from fact not found?
  expected_file: retrieval-agent-behavior.md
  expected_excerpt: "`fact not found` means the topic is inside it and the library does not hold the fact"

- case: positive
  question: How many libraries will kb-ask consult for one question at most, and when does it ask a second?
  expected_file: retrieval-agent-behavior.md
  expected_excerpt: "or for a question that needs facts from two libraries, never more than two"

- case: positive
  question: Did triage and specialists reach 95% routing at 48 libraries?
  expected_file: retrieval-agent-behavior.md
  expected_excerpt: "`kb-ask` scored 83% and today's agent 79%"

- case: positive
  question: A question spans three libraries. How many library-wide searches may kb-retrieve make before it stops?
  expected_file: retrieval-agent-behavior.md
  expected_excerpt: "three libraries do not get fifteen searches"

- case: positive
  question: Does kb-telemetry --not-found list the questions a specialist answered not mine?
  expected_file: retrieval-eval-integrity.md
  expected_excerpt: "is a routing miss, not a gap, and is not listed"

# Added 2026-11-09, engine-sync fold C: curation and guardrails.
- case: positive
  question: Should a fact file get a new section titled "Update 2026-11-04" when something it states changes?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "The date belongs on the `Verified:` stamp, and the history belongs in the note and in git"

- case: positive
  question: Does check-transient block a commit that adds a dated heading?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "a dated heading can be right (a dated release, a legal effective date), and only a person can say"

- case: positive
  question: A source URL redirects to a single sign-on login page that answers 200. Does kb-sources treat the source as alive?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "A login page answers 200, but it is not the source"

- case: positive
  question: After a library has been swept, which unstamped sources does the session-start nudge name?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "the commit that last changed last_swept in INDEX.md, read from git"

- case: positive
  question: Will the secret scanner block a JWT written as three dot-separated segments?
  expected_file: secret-governance.md
  expected_excerpt: "each one is random-looking, so it is still caught"

# Added 2026-11-09, distilled from the 2026-10-26 retest note.
- case: positive
  question: Does a kb-retrieve subagent's context carry the nudge that a library is behind the engine's contract?
  expected_file: retrieval-agent-behavior.md
  expected_excerpt: "the nudge names no library that is merely behind, in either context"

- case: positive
  question: Can a sampled retrieval run be graded while it is still running?
  expected_file: retrieval-eval-integrity.md
  expected_excerpt: "A grade of a run still in progress is a grade of a partial transcript"

# Added 2026-11-12, v0.4.0 defaults fold: kb-verify's default budget, kb-sources' timeout, closed packets.
- case: positive
  question: What verifier_budget does kb-verify use when a library INDEX.md does not declare one?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "gets a budget of 5, and"

- case: positive
  question: How long does kb-sources wait for a URL to respond before it gives up on it?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "each request has a 10-second timeout"

- case: positive
  question: How long are review packets kept after kb-apply marks them applied, before they are deleted?
  expected_file: unattended-preparation.md
  expected_excerpt: "where it is kept. Nothing in the engine deletes a closed packet"

# Added 2026-11-20, pre-release fold of the engine's Unreleased change notes and ADR-0021: fixed paths, local-only libraries, KB_TODAY, the nudge and the weekly report, what kb-verify reads, the one root INDEX.md reader.
- case: positive
  question: Why did the engine stop following XDG_CONFIG_HOME for where kb-schedule keeps its config?
  expected_file: design-thesis.md
  expected_excerpt: "ran with preparation off and without the PATH install had saved"

- case: positive
  question: How can a person keep the engine's review packets and weekly reports somewhere other than its default state folder?
  expected_file: design-thesis.md
  expected_excerpt: "A person who wants state elsewhere sets"

- case: positive
  question: A person had XDG_STATE_HOME set before the engine moved to fixed paths. What happens to their old packets and reports?
  expected_file: design-thesis.md
  expected_excerpt: "the engine no longer reads the old folders"

- case: positive
  question: What replaced the five copies of the state-folder rule that were spread across the engine's scripts?
  expected_file: design-thesis.md
  expected_excerpt: "replaces five copies of the state-folder rule"

- case: positive
  question: After upgrading, what must a person with a scheduled job do for CLAUDE_CONFIG_DIR to reach the scheduled run?
  expected_file: design-thesis.md
  expected_excerpt: "reinstall to record it"

- case: positive
  question: What checks a commit made with --no-verify in a library that has no git remote?
  expected_file: design-thesis.md
  expected_excerpt: "Its only gate is the commit hook"

- case: positive
  question: What does a library's links_to entry hold when the library it links into has no remote, and is that checked?
  expected_file: releases-and-contract-versioning.md
  expected_excerpt: "a path is a record of what was linked against, not a check"

- case: positive
  question: Does kb-library export record that a registered library is read-only?
  expected_file: releases-and-contract-versioning.md
  expected_excerpt: "now carry a library's read-only mode and pinned tag"

- case: positive
  question: A library's root INDEX.md repeats a key in its frontmatter. Which occurrence do the engine's scripts read?
  expected_file: design-thesis.md
  expected_excerpt: "On a repeated key the first occurrence wins"

- case: positive
  question: What happens when KB_TODAY is set to something that is not a date?
  expected_file: guardrail-verification.md
  expected_excerpt: "A malformed value exits 2 and names the variable; it never falls back to the real date."

- case: positive
  question: A library has more rechecks due than its verifier budget. Does the session-start nudge count all of them or only one run's worth?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "Within the budget the wording is unchanged"

- case: positive
  question: Does a pending kb-distill review packet hide a due sweep from the nudge?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "A packet covers only what its skill does: an audit packet does not hide the notes, nor a distill packet the sweep."

- case: positive
  question: Why did kb-due count a stamp-less Source line in a router as an unstamped source, and does it still?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "so a router counted"

- case: positive
  question: What does session start do with a kb.nudge value it does not recognise?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "warns in the session-start message"

- case: positive
  question: How does kb-verify treat a Verified stamp that names no by field?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "so the shorter agent TTL applies"

- case: positive
  question: Does rejecting a Recheck that the shell cannot parse need a contract version bump?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "It fails at every contract version, like an indented stamp"

- case: positive
  question: Which assertions does the WEAK check in kb-verify judge, now that containing a URL is not enough?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "It judges one that fetches it"

- case: positive
  question: Why did check-transient report a dated heading found in a library's Claude Code settings folder?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "which lacked those two"

- case: positive
  question: Is an archived library that is not registered still reported in the weekly report?
  expected_file: unattended-preparation.md
  expected_excerpt: "A registered library is still reported: it is served whatever it declares."

- case: positive
  question: When does kb-report still end with exit 2 now that one unreadable library no longer stops it?
  expected_file: unattended-preparation.md
  expected_excerpt: "Exit 2 stays for an engine that cannot load its readers, or a report folder inside a library."

- case: positive
  question: Does check-secrets --file give a non-md file under a library's documents folder the raw-artifact exemption?
  expected_file: secret-governance.md
  expected_excerpt: "gets the same raw-artifact exemption as"

- case: positive
  question: Why was a plain slug in a keywords list once flagged by check-secrets?
  expected_file: secret-governance.md
  expected_excerpt: "looked like a mixed-class token"

# Amended 2026-11-23: was a negative case; the changes after v0.4.0 shipped as v0.5.0 on 2026-11-23.
- case: positive
  question: When were the engine and this library released as v0.5.0?
  expected_file: releases-and-contract-versioning.md
  expected_excerpt: "v0.4.0 on 2026-11-12 and v0.5.0 on 2026-11-23"

- case: negative
  question: Who decided that the engine's state should live at fixed paths?
  expected_file: ""
  expected_excerpt: "fact not found — check KB"

# Added 2026-11-24, fold of the engine's Unreleased change notes (v0.6.0) and ADR-0022: the lease follows the evidence, contract 2.7, check-stamps retired, coverage in the nudge, what retrieval says backs a fact, expected_evidence.
- case: positive
  question: Does a stamp that says by human get a longer lease than one an agent wrote?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "a person's stamp buys no longer lease than an agent's"
  expected_evidence: check

- case: positive
  question: Is a library below full assertion coverage named in the session-start nudge when nothing else is due?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "Coverage is a measure, not something due, but a library below full coverage is named in the nudge even when nothing is due"
  expected_evidence: check

- case: positive
  question: Can a library that declares contract 2.7 still name its short lease agent_ttl?
  expected_file: releases-and-contract-versioning.md
  expected_excerpt: "A 2.7 library that still declares agent_ttl fails check-kb."
  expected_evidence: check

- case: positive
  question: What must a library that copied the guardrails template do before it moves its engine pin past v0.5.0?
  expected_file: script-reference-checks.md
  expected_excerpt: "A library that copied the template must delete its \"Stamp honesty\" step before it moves its engine pin"
  expected_evidence: check

- case: positive
  question: Does kb-retrieve refuse a fact because its section has a stamp but no check behind it?
  expected_file: retrieval-agent-behavior.md
  expected_excerpt: "never refuse, withhold or soften a fact for lacking a check"
  expected_evidence: check

- case: positive
  question: Why does retrieval say a fact carries a check and not that it is asserted?
  expected_file: contract-retrieval-and-verification.md
  expected_excerpt: "Retrieval cannot run the weak-assertion lint, so it says"
  expected_evidence: check

- case: positive
  question: What does session start say when kb-due fails?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "kb-due failed (exit N), so the knowledge-base nudge is unavailable this session."
  expected_evidence: check

# Added 2026-11-24, refresh after ai-kb #263 (a library behind on a supported contract is not a decision).
- case: positive
  question: Is a library built to contract 2.6, on an engine that knows 2.7, a decision in the weekly report or a clause in the nudge?
  expected_file: releases-and-contract-versioning.md
  expected_excerpt: "Since #263 it is not told by the session-start nudge and is not a decision in the weekly report"
  expected_evidence: check
