---
name: retrieval-eval-integrity
description: Why a retrieval eval can pass or fail for reasons that have nothing to do with the knowledge — an oracle the agent can reach, a grader that tests markup or strings or demands a quote nobody asked for, an oracle that follows the agent, discovery or a wrong corpus that fails silently, a protocol that moved between runs, a clean control nobody checked, one trial per case, a contamination rule broader or narrower than the contract, two readers of one record, and a golden set that never tests the boundary between neighboring libraries — and how each was found, and how retrieval cost and a sampled eval are measured from transcripts.
memory_type: semantic
domain: knowledge-architecture
scope: general
verified: 2026-10-12
metadata:
  type: fact
  node_type: memory
  created: 2026-10-02
tags: [knowledge-architecture, retrieval, eval, golden-set]
keywords: [retrieval eval, golden set, oracle, grader, grade-eval, grade-routing, transcripts, SubagentHandback, verbatim quote, exclusion glob, wrong corpus, library list unknown, contamination, alt_excerpt, alternative excerpt, negative case, clean control, gold key, post hoc amendment, protocol, trials, sample size, discovery, kb-retrieve, blind, contamination definition, documents/eval, single-file Grep, ripgrep, YAML scalar, unquote, boundary case, neighboring library, kb-telemetry, kb-eval-sample, tokens, transcripts]
---
# Retrieval eval integrity

A retrieval eval puts a question to an agent that holds no facts, then checks the answer against an oracle: the golden set (`ai-kb:CONVENTIONS.md` §10). Every serious fault found in the eval so far was in the apparatus around retrieval, not in the library being tested. Each section below is a way the result stopped meaning what it appeared to mean. The rules that came out of them live in the contract; this file holds the failure and the reason.

## The agent under test must not be able to reach the oracle

On the first run the golden set was routed from the domain router, so the agent descended to it and read the expected answers, and said so in its transcript. "Refusals obtained that way prove nothing: nobody can distinguish retrieval from recitation." The oracle is now unrouted, the agent is forbidden from opening one, and the sweep exempts it from the orphan check.

The same contamination returned in a new form on the second library. Eval evidence committed inside the library put earlier answers where a search could reach them, and one run's agent searched them up. It is now excluded exactly as the golden set is. Whether the oracle was touched is read from the agents' tool calls, not from their say-so: in one run two searches ran without the exclusion glob and returned filenames only, so the golden file's path showed and none of its content did; in the next run no agent touched the oracle, as the tool calls confirmed. A later run's agent strayed into the engine's plan document, outside the knowledge base, and used none of it in an answer.

A pre-judging sweep for files changed outside the review folders, and a ban on listing folders at all, were the run-integrity measures of the head-to-head comparison of two skills. One reviewer there listed a scratchpad folder, which showed file names including the gold key and no file contents. It reported this itself, and the run was counted because a file name gives away no answer.

The check for contamination can itself fail a compliant agent. The first routing run graded from tool
calls (2026-10-12) scored 6/12 at first grading, and all six failures were one grader bug: the agent is
told to give its searches an exclusion glob naming the golden set, and the grader matched "golden" in
the arguments, "so every compliant agent that followed the instruction failed." A call with a paired
result is now judged by the paths in its result alone. After the fix the run scored 11/12.

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§9.8, §9.14, §9.15, §9.16) · [[eval-2026-09-28-intelligence-analysis-bakeoff]] · [[eval-2026-10-12-routing-run]] · [[script-grade-routing]] · ai-kb@48467fd
Verified: 2026-10-16 · by: agent · method: doc-review

## Contamination is what the contract forbids, no more and no less

A routing grader that decides contamination by pattern fails a compliant agent as easily as it misses a violation. Contamination is what the retrieval contract forbids: reading or searching a golden set, or a path under a library's `documents/eval-*` folder. A first definition was broader: it matched a path component named `eval` or `evals`, or one starting with `eval` and a separator, so a routed citation stub under `sources/` named `eval-...` failed a case. The stub is a citation and not evidence. The grader now matches `-golden.md` files and `documents/eval-*` paths only.

Two more defects came from the grader reading the tool's output format as if it carried more than it does. When a Grep's path is a single file, ripgrep omits the file name from content lines, so no result line named a file and the grader could not tell which library the content came from. Such a result is now attributed to that file, and counts as content from that library and not as a Read of the file. And content-mode lines were split at the first digits between separators, so a `-<digits>-` segment inside a path, such as a session UUID in a checkout path, truncated the path: they are now split at the first extension followed by a line number.

Source: [[script-grade-routing]] · [[engine-changelog-unreleased]]
Verified: 2026-10-28 · by: agent · method: doc-review
Recheck: grep -qF 'documents/eval-[^/]*' "$KB_ENGINE_ROOT/scripts/grade-routing"

## A grader must test grounding, not formatting

The first run scored 5/6 and both discrepancies were defects in the eval. An expected excerpt carried markdown emphasis, text that exists in a file and never in a spoken answer, so a correct, grounded answer failed on a pair of asterisks. Excerpts are now markup-free prose, matched with whitespace normalised.

The grader still confused punctuation with grounding afterwards. It failed escaped, nested and curly quotes, and a quote that stopped before the excerpt's full stop, and it crashed outright on an answer missing a field. It now folds quote characters, ignores an excerpt's trailing punctuation, and fails a malformed record by name. A paraphrase still fails. Every earlier run, re-graded against its original oracle, kept its stored verdict.

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§9.8, §9.15)
Verified: 2026-10-12 · by: agent · method: doc-review

## An oracle must have one reader

Golden-set values are read as YAML scalars. A single-quoted excerpt, the natural way to write one holding `"` or `: `, was searched for with its quotes still on and could never match. Now `'…'` loses its quotes and reads `''` as `'`, `"…"` reads its escapes, and a plain value keeps a closing quote of its own. `grade-eval` imports `check-golden`'s reader instead of keeping a copy, so the two cannot drift apart again. No existing golden set read differently afterward.

Source: [[engine-changelog-unreleased]] · [[script-grade-eval]]
Verified: 2026-10-28 · by: agent · method: doc-review

## A grader must resolve identity, and demand only what the contract requires

The second library's eval went 9/16, 14/16, 11/16 and 15/16 across four runs, and every drop had a cause outside the library. Two were in the grader and contract, below, and a third is under library discovery. A later wave, on a contradiction, added the third item below.

- **The grader string-matched citations.** It compared `kb/<lib>/x.md` against `x.md`. It now resolves a cited path to a file and still rejects a same-named file in another library. Its first fix broke on a relative library path, and a one-point discrepancy between two gradings of the same answers is what exposed it. The grader had no regression cases until that wave.
- **The contract and the grader disagreed about refusals.** The agent was to cite every file used, while a negative case was to cite none. A citation now means the answer rests on this file, so a refusal cites nothing and names what it checked in its text.
- **The oracle demanded a phrase nothing required the agent to say.** On a contradiction the agent served nothing, stated both claims and cited the file, and still failed because the oracle expected the literal `status: CONFLICTED`. The refusal is now fixed as "disputed — not served", then both claims, citing the conflicted file, which makes it the one refusal that cites. `check-golden` now requires an unresolved case to name a file that really holds a `CONFLICTED` section. In the next run the case passed.
- **The grader demanded a quote the agent was never asked for.** `grade-eval` passes a positive case only if the expected excerpt appears in the answer, and the agent "was told to cite but never to quote, so correct, grounded, cited answers failed whenever they paraphrased. One pilot scored 6/11." The agent now quotes, for each value an answer depends on, "the sentence from the file that states it, word for word, in quotation marks."
- **The grader read the wrong message.** Grading a sub-agent from its transcript, "The obvious extractor reads the last text block, but a subagent's answer arrives in its `SubagentHandback` call, so that extractor grades the agent's preamble." `grade-eval --transcripts` reads the hand-back. In the change's live sample of 16 questions from one library, the first grading scored 13/16, and all three failures were notes the agent had added to its `Sources:` line; with the grader dropping those notes, it scored 16/16. That is one run, reported by the change's author.

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§9.14, §9.15) · [[pr-54-kb-retrieve-quotes]] · [[agent-kb-retrieve]] · [[script-grade-eval]] · ai-kb@510d09b · ai-kb@d978338 · ai-kb@43aee00
Verified: 2026-10-16 · by: agent · method: doc-review

## A file may state a fact twice, and an oracle that follows the agent measures nothing

The oracle accepts one sentence per fact, and a file may state a fact twice, usually a summary line and the source it quotes. Across the first three new libraries, most genuine misses were the agent quoting a different sentence from the right file that supports the same fact. One library's question failed this way in two runs, quoting the same alternative sentence each time.

The plan first recorded whether a golden record should accept alternative excerpts as an open decision. It was taken on 2026-09-24: a record may name `alt_excerpt`, and the grader passes an answer quoting any listed excerpt while still requiring every one to be in the file (ai-kb@3bbd400; rules in `ai-kb:CONVENTIONS.md` §10). On the first library evaluated on a protocol that held still, `alt_excerpt` was judged to reduce the pattern but not eliminate it: "adding alternatives after each run would make the oracle follow the agent", so none was added. An alternative added after results were seen is an amendment and is recorded as one, as is a shortened excerpt: in one run an excerpt was shortened because two independent agents quoted around it, and the change is recorded as post hoc in the golden file and the evidence README.

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§9.14, §9.15, §9.16) · ai-kb@3bbd400
Verified: 2026-10-12 · by: agent · method: doc-review

## Library discovery must be exact, and checked from tool calls

An agent that has not been given the list of installed libraries must find them itself, and in the third run of one eval the first listing of `kb/*` was swamped by one library's `.git` objects. One agent concluded that library was the only one installed and refused four answerable questions with confidence. Enumerating `kb/*/INDEX.md`, one router per library, was exact, and the fourth run was checked from the agents' tool calls, not from what they said: all four agents had used that glob.

A wrong corpus fails the same way. An agent not told where the engine is could find some other knowledge tree instead: "a workspace can hold an older one, and an answer from it looks exactly like a correct one. That happened — 25 of 25 sub-agents answered from the wrong corpus, and nothing reported it." The agent now stops and answers `engine root unknown — not searched` when its context does not name the engine.

The enumeration was replaced when libraries became registered per machine (`ai-kb:CONVENTIONS.md` §3). The agent treats the libraries its context lists as the complete set: "If your context names the engine but lists no libraries, stop and answer `library list unknown — not searched`." It never globs the engine's `kb/` for them, because "it would find only the libraries that happen to sit there, and a partial set looks exactly like a complete one."

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§9.14) · [[agent-kb-retrieve]] · [[adr-0014-library-registry]] · ai-kb@48467fd
Verified: 2026-10-16 · by: agent · method: doc-review

## A result is about the protocol that held still

Every drop across the four runs above had a cause outside the library. What the corrected protocol established: agents with no knowledge of the answers routed every positive question to the right file and refused every question the library deliberately cannot answer. What it did not: a clean pass, or anything about a protocol that changed between runs rather than one repeated.

The first library evaluated after the eval stopped changing scored 16/19, and that measures the knowledge and the agent, not the apparatus. No defect was found in the grader or the contract. Two of the three misses were the one-sentence pattern above. The third was a refusal that explained itself: the agent refused, then cited the files that showed why no answer exists, and paraphrased the fixed phrase. The contract already covers it (a refusal names what it checked in its text and cites nothing), so it is a miss, not a gap. The remaining misses were not patched one run at a time.

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§9.14, §9.16, §9.17)
Verified: 2026-10-12 · by: agent · method: doc-review

## A clean control is a claim

A behavioural eval of a skill over a library used six cases from a graded corpus, three of them clean controls with a gold key. Flawed products passed 3/3 and clean controls failed 0/3 in rounds 3 and 4: every control drew two or three findings. Editing the rule text did not converge, because each wording change moved the false positives rather than removing them. A structural change (load library files only when a surviving finding needs one) was tested and also scored 3/6.

The tell was that independent reviewers under three versions of the skill converged on the same specific issues in each control, which is not a checklist effect. Either the reviewers' bar sat below the corpus author's, or the controls were less clean than their keys said. The owner read the seven recurring findings against the quoted passages and judged all seven real weaknesses. The gold key was amended post hoc, with the original kept and the amendment recorded, and against the amended key rounds 3 and 4 each score 6/6. The recorded grades stay against the original key. The amendment had one adjudicator reading findings framed by the agent that ran the eval, so a second reader would make it sturdier.

What generalises: a clean control is a claim, and it needs the same scrutiny as a planted flaw. When independent reviewers converge on the same "false positive", check the key before the reviewer.

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§9.17) · [[eval-2026-09-26-intelligence-analysis-skill]] · ai-kb@889ab0b
Verified: 2026-10-12 · by: agent · method: doc-review

## One trial per case shows direction, not frequency

Each case in both skill evals ran once per round, and the plan says what that buys: the results "say which way things point, not how often they happen". In the head-to-head comparison of a rebuilt skill with the legacy one, the legacy skill won 6–3–1 on ten held-out cases and the rebuilt one won 4–2 on the remaining six, with a revised skill version and different cases in each round, so the two rounds do not pool into one score. Two biases pulled in opposite directions: the legacy skill was built and tuned on the corpus, and the rebuilt skill's last wording was written after seeing two of the final cases. A test with no home advantage needs cases neither skill has seen.

The routing sample behind the decision not to build an orchestrator had the same limit: 12 questions, three agents answering four each, graded by hand against a key; its limits are stated under "Orchestrator cost bounds" in [[agent-lifecycle-roles]] and are not restated here.

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§9.17) · [[eval-2026-09-28-intelligence-analysis-bakeoff]] · [[eval-2026-09-28-routing-sample]]
Verified: 2026-10-12 · by: agent · method: doc-review

## A golden set that never asks across the boundary does not test where routing fails

The engine's issue proposing boundary cases records that the routing failures that mattered were questions near the line between two libraries, and that the evals barely tested that line, since only a handful of cases were `cross-root`. A boundary case is a question whose wording fits a neighboring library but whose answer lives in the library being tested. The stated rule is two to four per library, written by someone who has not seen the routers' wording, and the stated acceptance is at least two per library with `check-golden` passing.

Source: [[issue-126-boundary-golden-cases]]
Verified: 2026-10-28 · by: agent · method: doc-review

## Cost is measured from transcripts already on disk, and a sample is graded from the same transcripts

Claude Code records each subagent run as a transcript with a metadata file naming its agent type and description. `kb-telemetry` reads the `kb-retrieve` runs from those and reports, per library and window, the median of five measures:

- **tokens:** new tokens, meaning input, cache-creation and output. One API message is logged in several records, so each message id is counted once. Cache reads are left out: they are the cheap re-reading of what is already cached, not new work;
- **seconds:** from the run's first API response to the first read of a file the answer cites. The spawn is not the start, because its queue time would dominate;
- **steps:** tool calls up to and including that read;
- **wasted:** content files read that the answer does not cite;
- **protocol:** reads of routers, contradictions, open-questions and queue files, counted apart from waste because the contract asks for them.

A window with no runs still gets a row, because absence is a result, and windows are contiguous so a reshape of a library can be compared before and after. Eval runs are left out by their description. It is read-only and prints paths, counts and medians, never a question, an answer or a file's text.

Normal use does not say whether answers were right, so `kb-eval-sample` draws a seeded sample of a library's positive and cross-root golden cases, has a person put each to a fresh agent, and grades the runs with `grade-routing` and `grade-eval --transcripts`, joining each case with its cost measured the same way. The agents must be spawned in a session started after the change being measured, so they run the current definition. A random sample can miss what a reshape changed, so `--cases` draws exactly the cases named, for a before-and-after comparison.

Source: [[script-kb-telemetry]] · [[script-kb-eval-sample]]
Verified: 2026-10-28 · by: agent · method: doc-review
