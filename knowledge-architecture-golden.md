---
name: knowledge-architecture-golden
description: Golden retrieval set for the knowledge-architecture domain — routing and answer-grounding cases for design-thesis, agent-lifecycle-roles, secret-governance, guardrail-verification and retrieval-eval-integrity.
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
tests that pass for the wrong reason). Cross-root case: included — the
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
  expected_file: agent-lifecycle-roles.md
  expected_excerpt: "nothing happens unless a session starts"

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
```
