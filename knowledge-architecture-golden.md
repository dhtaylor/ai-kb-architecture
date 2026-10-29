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
cost and sample tooling). Cross-root case: included — the
domain holds two cross-library references into claude-code-runtime, one from design-thesis
(`[[claude-code-runtime:behaviour-loading]]`) and one from guardrail-verification
(`[[claude-code-runtime:config-resolution]]`), and a `cross-root` record below tests each. Unresolved
case: **not applicable** — nothing in the domain today carries `status: CONFLICTED`, so a genuine
UNRESOLVED case cannot yet be written without fabricating a conflict. Add one the day a real
contradiction lands.

```yaml
- case: positive
  question: What is the core design rule for combining a governed KB with an agent layer?
  expected_file: design-thesis.md
  expected_excerpt: "separate knowledge from behavior, and let the behavior layer keep the knowledge layer honest"

- case: positive
  question: What are the three librarian roles that keep this kind of KB current, and which one splits into two?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "Curator (cataloguer)"

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

- case: positive
  question: What does an agent cite when it refuses to answer, and where does it say what it checked?
  expected_file: retrieval-eval-integrity.md
  expected_excerpt: "so a refusal cites nothing and names what it checked in its text"

- case: positive
  question: Why did an agent that handled a disputed fact correctly still fail the eval?
  expected_file: retrieval-eval-integrity.md
  expected_excerpt: "still failed because the oracle expected the literal"

- case: positive
  question: Why not add alternative excerpts to the oracle after each run's misses?
  expected_file: retrieval-eval-integrity.md
  expected_excerpt: "adding alternatives after each run would make the oracle follow the agent"
  alt_excerpt: "An alternative added after results were seen is an amendment and is recorded as one"

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

- case: positive
  question: Why did a secret scanner print its findings and still exit 0?
  expected_file: guardrail-verification.md
  expected_excerpt: "so the counter it incremented lived in a subshell and was discarded"
  alt_excerpt: "worse than no guardrail because it reads as protection"

- case: positive
  question: Why would the first draft of the session-start nudge have been ignored?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "so it would have asked about them at every session start, forever"

- case: positive
  question: What did the first draft of the session-start hook cost, and why?
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "took the hook from 0.06s to 3s"

- case: positive
  question: As built, does the Verifier query live systems to re-verify a fact?
  expected_file: design-thesis.md
  expected_excerpt: "re-runs an assertion the fact carries beside it"

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

- case: cross-root
  question: Where is the shell-startup behaviour that made that hook skip its checks recorded?
  expected_file: guardrail-verification.md
  expected_excerpt: "The shell-startup behaviour itself is recorded in"

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

- case: positive
  question: Does kb-bootstrap change a machine's configuration when run without an apply flag?
  expected_file: guardrail-verification.md
  expected_excerpt: "It now reports and changes nothing unless given"

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

- case: positive
  question: Why must a retrieval agent given no library list not search the engine's kb folder for libraries?
  expected_file: retrieval-eval-integrity.md
  expected_excerpt: "a partial set looks exactly like a complete one"

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

# Engine-sync fold: owner-approved scope 2026-10-28

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

- case: positive
  question: "Does anything keep the libraries current unattended by default?"
  expected_file: "unattended-preparation.md"
  expected_excerpt: "Nothing runs unless the user installs it"

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
  expected_excerpt: "It never holds a fact's, a source's or a packet's text"

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
  question: "Is the engine's decision record part of the knowledge base the retrieval agent may answer from?"
  expected_file: "retrieval-agent-behavior.md"
  expected_excerpt: "The engine repository is not part of it."

- case: positive
  question: "How does the retrieval agent read a file once the routers have picked it?"
  expected_file: "retrieval-agent-behavior.md"
  expected_excerpt: "read from that heading's line to the next heading at the same level or higher"

- case: positive
  question: "When does the retrieval agent read a whole file instead of one section?"
  expected_file: "retrieval-agent-behavior.md"
  expected_excerpt: "when two fit and it cannot tell which, or when the answer needs more than one section"

- case: positive
  question: "Why is every search scoped to the library the routers chose?"
  expected_file: "retrieval-agent-behavior.md"
  expected_excerpt: "because a search across every library reads text from libraries the question does not need"

- case: positive
  question: "What does the retrieval agent do with a library whose scope line excludes the question?"
  expected_file: "retrieval-agent-behavior.md"
  expected_excerpt: "It does not descend into a library whose line excludes the question's topic"

- case: positive
  question: "Does a scope line make the retrieval agent search every library it names?"
  expected_file: "retrieval-agent-behavior.md"
  expected_excerpt: "It does not widen the search to every library it names"

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
  question: "A registered library is not served and a question needs it. What does the agent say?"
  expected_file: "retrieval-agent-behavior.md"
  expected_excerpt: "the gap is a broken install and not missing knowledge"

- case: positive
  question: "An agent answered a question the library cannot answer by reading an engine decision record. Which of its beliefs was wrong?"
  expected_file: "retrieval-agent-behavior.md"
  expected_excerpt: "The answering agent was right about the record and wrong about the knowledge base"

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
  question: "What states can a registered library be in, beyond missing, not a library and name mismatch?"
  expected_file: "design-thesis.md"
  expected_excerpt: "The fifth state is a library whose declared contract version names a major the engine does not support, or cannot be read"

- case: positive
  question: "What ties a library to the engine's rules, if not a pin on the engine?"
  expected_file: "design-thesis.md"
  expected_excerpt: "What ties a library to the engine's rules is the contract version it declares, not a pin on the engine's code"
```
