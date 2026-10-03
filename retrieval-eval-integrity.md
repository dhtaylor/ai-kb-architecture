---
name: retrieval-eval-integrity
description: Why a retrieval eval can pass or fail for reasons that have nothing to do with the knowledge — an oracle the agent can reach, a grader that tests markup or strings, an oracle that follows the agent, discovery that fails silently, a protocol that moved between runs, a clean control nobody checked, and one trial per case — and how each was found.
memory_type: semantic
domain: knowledge-architecture
scope: general
verified: 2026-10-02
metadata:
  type: fact
  node_type: memory
  created: 2026-10-02
tags: [knowledge-architecture, retrieval, eval, golden-set]
keywords: [retrieval eval, golden set, oracle, grader, grade-eval, contamination, alt_excerpt, alternative excerpt, negative case, clean control, gold key, post hoc amendment, protocol, trials, sample size, discovery, kb-retrieve, blind]
---
# Retrieval eval integrity

A retrieval eval puts a question to an agent that holds no facts, then checks the answer against an oracle: the golden set (`ai-kb:CONVENTIONS.md` §10). Every serious fault found in the eval so far was in the apparatus around retrieval, not in the library being tested. Each section below is a way the result stopped meaning what it appeared to mean. The rules that came out of them live in the contract; this file holds the failure and the reason.

## The agent under test must not be able to reach the oracle

On the first run the golden set was routed from the domain router, so the agent descended to it and read the expected answers, and said so in its transcript. "Refusals obtained that way prove nothing: nobody can distinguish retrieval from recitation." The oracle is now unrouted, the agent is forbidden from opening one, and the sweep exempts it from the orphan check.

The same contamination returned in a new form on the second library. Eval evidence committed inside the library put earlier answers where a search could reach them, and one run's agent searched them up. It is now excluded exactly as the golden set is. Whether the oracle was touched is read from the agents' tool calls, not from their say-so: in one run two searches ran without the exclusion glob and returned filenames only, so the golden file's path showed and none of its content did; in the next run no agent touched the oracle, as the tool calls confirmed. A later run's agent strayed into the engine's plan document, outside the knowledge base, and used none of it in an answer.

A pre-judging sweep for files changed outside the review folders, and a ban on listing folders at all, were the run-integrity measures of the head-to-head comparison of two skills. One reviewer there listed a scratchpad folder, which showed file names including the gold key and no file contents. It reported this itself, and the run was counted because a file name gives away no answer.

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§9.8, §9.14, §9.15, §9.16) · [[eval-2026-09-28-intelligence-analysis-bakeoff]] · ai-kb@48467fd
Verified: 2026-10-02 · by: agent · method: doc-review

## A grader must test grounding, not formatting

The first run scored 5/6 and both discrepancies were defects in the eval. An expected excerpt carried markdown emphasis, text that exists in a file and never in a spoken answer, so a correct, grounded answer failed on a pair of asterisks. Excerpts are now markup-free prose, matched with whitespace normalised.

The grader still confused punctuation with grounding afterwards. It failed escaped, nested and curly quotes, and a quote that stopped before the excerpt's full stop, and it crashed outright on an answer missing a field. It now folds quote characters, ignores an excerpt's trailing punctuation, and fails a malformed record by name. A paraphrase still fails. Every earlier run, re-graded against its original oracle, kept its stored verdict.

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§9.8, §9.15)
Verified: 2026-10-02 · by: agent · method: doc-review

## A grader must resolve identity, and demand only what the contract requires

The second library's eval went 9/16, 14/16, 11/16 and 15/16 across four runs, and every drop had a cause outside the library. Two were in the grader and contract, below, and a third is under library discovery. A later wave, on a contradiction, added the third item below.

- **The grader string-matched citations.** It compared `kb/<lib>/x.md` against `x.md`. It now resolves a cited path to a file and still rejects a same-named file in another library. Its first fix broke on a relative library path, and a one-point discrepancy between two gradings of the same answers is what exposed it. The grader had no regression cases until that wave.
- **The contract and the grader disagreed about refusals.** The agent was to cite every file used, while a negative case was to cite none. A citation now means the answer rests on this file, so a refusal cites nothing and names what it checked in its text.
- **The oracle demanded a phrase nothing required the agent to say.** On a contradiction the agent served nothing, stated both claims and cited the file, and still failed because the oracle expected the literal `status: CONFLICTED`. The refusal is now fixed as "disputed — not served", then both claims, citing the conflicted file, which makes it the one refusal that cites. `check-golden` now requires an unresolved case to name a file that really holds a `CONFLICTED` section. In the next run the case passed.

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§9.14, §9.15) · ai-kb@510d09b · ai-kb@d978338 · ai-kb@43aee00
Verified: 2026-10-02 · by: agent · method: doc-review

## A file may state a fact twice, and an oracle that follows the agent measures nothing

The oracle accepts one sentence per fact, and a file may state a fact twice, usually a summary line and the source it quotes. Across the first three new libraries, most genuine misses were the agent quoting a different sentence from the right file that supports the same fact. One library's question failed this way in two runs, quoting the same alternative sentence each time.

The plan first recorded whether a golden record should accept alternative excerpts as an open decision. It was taken on 2026-09-24: a record may name `alt_excerpt`, and the grader passes an answer quoting any listed excerpt while still requiring every one to be in the file (ai-kb@3bbd400; rules in `ai-kb:CONVENTIONS.md` §10). On the first library evaluated on a protocol that held still, `alt_excerpt` was judged to reduce the pattern but not eliminate it: "adding alternatives after each run would make the oracle follow the agent", so none was added. An alternative added after results were seen is an amendment and is recorded as one, as is a shortened excerpt: in one run an excerpt was shortened because two independent agents quoted around it, and the change is recorded as post hoc in the golden file and the evidence README.

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§9.14, §9.15, §9.16) · ai-kb@3bbd400
Verified: 2026-10-02 · by: agent · method: doc-review

## Library discovery must be exact, and checked from tool calls

An agent that has not been given the list of installed libraries must find them itself, and in the third run of one eval the first listing of `kb/*` was swamped by one library's `.git` objects. One agent concluded that library was the only one installed and refused four answerable questions with confidence. The contract now enumerates `kb/*/INDEX.md`, one router per library, which is exact (`ai-kb:CONVENTIONS.md` §3). The fourth run was checked from the agents' tool calls, not from what they said, and all four had used that glob.

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§9.14) · ai-kb@48467fd
Verified: 2026-10-02 · by: agent · method: doc-review

## A result is about the protocol that held still

Every drop across the four runs above had a cause outside the library. What the corrected protocol established: agents with no knowledge of the answers routed every positive question to the right file and refused every question the library deliberately cannot answer. What it did not: a clean pass, or anything about a protocol that changed between runs rather than one repeated.

The first library evaluated after the eval stopped changing scored 16/19, and that measures the knowledge and the agent, not the apparatus. No defect was found in the grader or the contract. Two of the three misses were the one-sentence pattern above. The third was a refusal that explained itself: the agent refused, then cited the files that showed why no answer exists, and paraphrased the fixed phrase. The contract already covers it (a refusal names what it checked in its text and cites nothing), so it is a miss, not a gap. The remaining misses were not patched one run at a time.

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§9.14, §9.16, §9.17)
Verified: 2026-10-02 · by: agent · method: doc-review

## A clean control is a claim

A behavioural eval of a skill over a library used six cases from a graded corpus, three of them clean controls with a gold key. Flawed products passed 3/3 and clean controls failed 0/3 in rounds 3 and 4: every control drew two or three findings. Editing the rule text did not converge, because each wording change moved the false positives rather than removing them. A structural change (load library files only when a surviving finding needs one) was tested and also scored 3/6.

The tell was that independent reviewers under three versions of the skill converged on the same specific issues in each control, which is not a checklist effect. Either the reviewers' bar sat below the corpus author's, or the controls were less clean than their keys said. The owner read the seven recurring findings against the quoted passages and judged all seven real weaknesses. The gold key was amended post hoc, with the original kept and the amendment recorded, and against the amended key rounds 3 and 4 each score 6/6. The recorded grades stay against the original key. The amendment had one adjudicator reading findings framed by the agent that ran the eval, so a second reader would make it sturdier.

What generalises: a clean control is a claim, and it needs the same scrutiny as a planted flaw. When independent reviewers converge on the same "false positive", check the key before the reviewer.

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§9.17) · [[eval-2026-09-26-intelligence-analysis-skill]] · ai-kb@889ab0b
Verified: 2026-10-02 · by: agent · method: doc-review

## One trial per case shows direction, not frequency

Each case in both skill evals ran once per round, and the plan says what that buys: the results "say which way things point, not how often they happen". In the head-to-head comparison of a rebuilt skill with the legacy one, the legacy skill won 6–3–1 on ten held-out cases and the rebuilt one won 4–2 on the remaining six, with a revised skill version and different cases in each round, so the two rounds do not pool into one score. Two biases pulled in opposite directions: the legacy skill was built and tuned on the corpus, and the rebuilt skill's last wording was written after seeing two of the final cases. A test with no home advantage needs cases neither skill has seen.

The routing sample behind the decision not to build an orchestrator had the same limit: 12 questions, three agents answering four each, graded by hand against a key; its limits are stated under "Orchestrator cost bounds" in [[agent-lifecycle-roles]] and are not restated here.

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§9.17) · [[eval-2026-09-28-intelligence-analysis-bakeoff]] · [[eval-2026-09-28-routing-sample]]
Verified: 2026-10-02 · by: agent · method: doc-review
