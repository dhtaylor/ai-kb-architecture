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
cost and sample tooling).

Cross-root case: included — the domain holds three cross-library references into claude-code-runtime:
the scope line of the root router (`[[claude-code-runtime:behaviour-loading]]`), design-thesis
(`[[claude-code-runtime:behaviour-loading]]`) and guardrail-verification
(`[[claude-code-runtime:config-resolution]]`). Two `cross-root` records below test the hops from
design-thesis and from guardrail-verification; the scope line is routing signal and has no record of
its own. Unresolved case: **not applicable** — nothing in the domain today carries `status: CONFLICTED`, so a genuine
UNRESOLVED case cannot yet be written without fabricating a conflict. Add one the day a real
contradiction lands.

```yaml
- case: positive
  question: What is the core design rule for combining a governed KB with an agent layer?
  expected_file: design-thesis.md
  expected_excerpt: "separate knowledge from behavior, and let the behavior layer keep the knowledge layer honest"

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

- case: negative
  question: What is the production database's deploy target for this workspace?
  expected_file: ""
  expected_excerpt: "fact not found — check KB"

- case: negative
  question: What did the maintainer's personal debugging session on Tuesday conclude?
  expected_file: ""
  expected_excerpt: "fact not found — check KB"

- case: cross-root
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
  expected_file: secret-governance.md
  expected_excerpt: "is not restated here"

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
- case: cross-root
  question: A pre-commit hook fired from a non-interactive shell skipped its checks because the engine path was exported in .bashrc. Why does that shell not see the variable?
  expected_file: claude-code-runtime:config-resolution.md
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
  expected_file: "retrieval-agent-behavior.md"
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
  expected_excerpt: "The fifth state is a library whose declared contract version names a major the engine does not support, or cannot be read"

- case: positive
  question: "What ties a library to the engine's rules, if not a pin on the engine?"
  expected_file: "releases-and-contract-versioning.md"
  expected_excerpt: "Rules apply by the declared version."

- case: positive
  question: "Is retrieval as triage plus specialists built?"
  expected_file: "knowledge-architecture-open-questions.md"
  expected_excerpt: "it is not built and not decided: retrieval as triage plus specialists"
```
