---
name: retrieval-agent-behavior
description: How the retrieval agent decides where to look and what counts as an answer's source — what the knowledge base is and why the engine repository is not part of it, why it reads the section that answers and not the whole file, why a search is scoped to the routed library, how it picks between neighboring libraries with the scope line, why registration warns of an overlap, how a subagent is given the engine and library list, how a question goes through a triage desk to one library's specialist and why that was adopted (ADR-0018, Proposed), and how a search lists files before lines and stops when it stops paying.
memory_type: semantic
domain: knowledge-architecture
scope: general
verified: 2026-10-28
metadata:
  type: fact
  node_type: memory
  created: 2026-10-28
tags: [knowledge-architecture, retrieval, routing, kb-retrieve]
keywords: [kb-retrieve, rule 3, off-KB, section read, outline, Grep scope, search is not a router, Not covered here, scope line, neighboring libraries, overlap warning, kb-library add, keywords, tags, SubagentStart, kb-session-start, library list, unsupported-contract, routers, triage, kb-ask, kb-catalog, specialist, SPECIALIST, not mine, Referred to, ADR-0018, files_with_matches, stop rule, search limit, 8 reads, 5 searches, tally]
---
# Retrieval agent behavior

The retrieval contract (`ai-kb:CONVENTIONS.md` §10) says what an answer must contain. This file holds the choices in how the agent gets there: where it may look, how much it reads, and how it chooses among libraries. Each is a response to a failure seen in use.

## The knowledge base is the registered libraries and a project's tree, and nothing else

The agent's third rule says what the knowledge base is: the libraries its context lists and a project's own `knowledge/` tree. The engine repository is not part of it. Its decision records, eval records, plans and scripts describe how the knowledge base works, not what it knows, and its contract tells the agent how to retrieve and never what to answer. A decision record that mentions the topic is not an answer. When the libraries are silent the answer is `fact not found`, whatever a file outside them says.

The stated reason: "An answer taken from `.git/config` may be right, but it tells nobody whether the knowledge base holds the fact, and a gap nobody sees is a gap nobody fills." Reading the source is the caller's job.

The rule was added after a sample found a retrieval agent answering a question the library cannot answer from an engine decision record, to which a library file only points. The routing grader passed that case at first; how it now grades such a read is in [[retrieval-eval-integrity]]. After the change, three fresh agents asked the failing question each answered `fact not found`, named the engine record as the place the caller could check, and did not open it; three trials of one question show the rule is followed for the question that failed, not that the failure cannot recur.

Source: [[agent-kb-retrieve]] · [[script-grade-routing]] · [[eval-2026-10-23-section-reads]]
Verified: 2026-10-28 · by: agent · method: doc-review

## The agent reads the section that answers, not the whole file

Once the routers have picked a file, the agent finds the part that answers before reading it:

1. Outline the file: a Grep of that one file for its headings, with line numbers.
2. Pick the heading whose title fits the question, and read from that heading's line to the next heading at the same level or higher. Its subsections come with it.
3. Read the whole file instead when no heading fits, when two fit and it cannot tell which, or when the answer needs more than one section. If the section read does not answer, read the next likeliest or the whole file, and never answer from a section that does not hold the fact.

A section read carries everything the contract needs, because a status marker applies to the heading it sits directly under and a `Verified:` stamp belongs to its own section (`ai-kb:CONVENTIONS.md` §7, §8). Both are inside the range read. A `#` line inside a code block is not a heading, so a range that ends mid-block, or before the section's `Source:` and `Verified:` lines, is read on to the next real heading.

A sample taken after the change found every answer that rested on a file read as a section, except that the disputed case also read the contradictions register whole, and a disputed section still refused, because the section read held its CONFLICTED marker. That sample could not measure the token saving: its libraries have small files, and the large-file library on which the saving was first measured was on another machine.

Source: [[agent-kb-retrieve]] · [[eval-2026-10-23-section-reads]]
Verified: 2026-10-28 · by: agent · method: doc-review

## A search is not a router

The agent reads the routers first and searches only inside the library they sent it to. Every Grep takes a `path` of that library, never a folder holding several libraries and never the engine root, because a search across every library reads text from libraries the question does not need, which is what routing exists to prevent. If the routers point nowhere, the answer is `fact not found` and not a search of everything. A sample in which two agents searched across the libraries' parent folder before reading a router matched only the one library each needed, so neither case failed on it, and it is the pattern the owner had ruled a failure when the matches reached other libraries. After the off-KB change the pattern did not recur in the re-run, which was three trials of one question: that shows it did not repeat in those three, not that it cannot recur.

Source: [[agent-kb-retrieve]] · [[eval-2026-10-23-section-reads]]
Verified: 2026-10-28 · by: agent · method: doc-review

## Neighboring libraries are separated by a scope line, and registration warns of an overlap

Routing fails at the boundary between neighboring libraries. When two or more listed libraries could plausibly hold the answer, the agent loads each one's root `INDEX.md` and reads its `Not covered here:` line, if it has one (contract 2.3, see [[releases-and-contract-versioning]]). It does not descend into a library whose line excludes the question's topic, and goes to the library the line names instead, if that one is in its list. A library with no line is not excluded by that: it is judged by its hooks. The line only picks among libraries. It does not widen the search to every library it names, so there is no fan-out across libraries. The line names topics and never states facts about them, because it is routing signal.

`kb-library add` helps an author find the neighbor before routing does. It compares the new library's root `keywords:` and `tags:` with every other registered library's, and warns when two libraries share at least 3 terms and those terms are at least a quarter of the smaller library's terms. Terms are normalized (lowercase, runs of space, underscore and hyphen read as one hyphen, a trailing plural `s` dropped), and each library's own domain name and a list of generic words are removed first. The test uses the overlap against the smaller set and not Jaccard, because Jaccard hides a small library sitting inside a big neighbor's vocabulary, which is the pair routing confuses, and the count of 3 stops two short lists matching on one word. The warning names the neighbor and the shared terms, suggests a `Not covered here` line in each root `INDEX.md` and a rerun of the routing sample, and never refuses. It never changes the exit status and writes nothing. `kb-create-domain` asks for the line when a library is created.

Source: [[agent-kb-retrieve]] · [[script-kb-library]] · [[engine-changelog-v0.3.0]] · [[conventions-contract-versions]]
Verified: 2026-10-28 · by: agent · method: doc-review

## A retrieval subagent is given the engine and the library list by a hook

A subagent never sees the session-start context, and without the engine path a retrieval agent searched whatever other knowledge tree its workspace held. `kb-session-start --subagent` produces the same context as a SubagentStart event for `kb-retrieve`: the engine path, the registered libraries by absolute path, and each registered library that cannot be served, named with its state and path. The librarian's due-nudge is left out, because it is for the person's session and not for a retrieval. The agent treats the listed libraries as the complete set and answers `engine root unknown — not searched` or `library list unknown — not searched` when its context lacks them; why is in [[retrieval-eval-integrity]], and how libraries are registered is in [[design-thesis]].

A library listed as registered but not served is reported as unavailable, and never as `fact not found`, when the question needs it: the gap is a broken install and not missing knowledge. A library on an unsupported contract major is one such case; see [[releases-and-contract-versioning]].

Source: [[script-kb-session-start]] · [[agent-kb-retrieve]] · [[script-kb-libraries]]
Verified: 2026-10-28 · by: agent · method: doc-review

## A question goes to a triage desk, which sends it to one library's specialist

The default way to answer from the knowledge base is the `kb-ask` skill, a triage desk that runs in the main session. The decision that built it is **Proposed, not accepted** (below). The desk reads one catalog, printed by `kb-catalog`: each registered library's name, scope, description, `Not covered here:` line and router topics. It is built on demand from the root `INDEX.md` files, "so it cannot go stale", and a registered library that is not served is named, not dropped. The desk then starts one `kb-retrieve` specialist confined to one library.

- **Specialist mode** switches on when a prompt begins `SPECIALIST: <library>`. The specialist reads only that library: "Do not open, Glob or Grep any other library, not even its `INDEX.md`." It reads the library's root `INDEX.md` first, and if the topic is outside the library it stops there and answers `not mine`, naming under `Referred to:` the library its `Not covered here:` line names, if any. "`not mine` means the topic is outside this library. `fact not found` means the topic is inside it and the library does not hold the fact." A link into another library is named under `Referred to:`, not followed: "The caller decides whether to ask that library." The rest of the retrieval contract is unchanged.
- **The desk** asks a second library only after `not mine` or `fact not found` (the referred library, or its runner-up), or for a question that needs facts from two libraries, never more than two. It passes answers back word for word, and flags two answers to one question as an overlap rather than choosing one. "It never answers from the catalog or from memory." It declares a `triage` tool tier, Bash and Agent: the desk reads no library, so it declares no Read.
- **`kb-retrieve` called directly**, with no `SPECIALIST:` line, still does its own triage as before. Its description calls it the specialist that `kb-ask` starts, to be called directly only when the library is already known.

Source: [[adr-0018-triage-and-specialists]] · [[agent-kb-retrieve]]
Verified: 2026-11-09 · by: agent · method: doc-review

## Triage was adopted for the citations it delivers, not for routing

The decision records that the reopen trigger of the no-orchestrator decision ([[agent-lifecycle-roles]], "Orchestrator cost bounds") had been met. Routing dropped below 95%: the full 138-case run scored 93.5% after the grader fix, and the 2026-10-26 retest 94.2%. More libraries made it worse: the 24-case subsample fell from 22/24 at 6 libraries to 18/24 at 48. And "Neighbors caused every failure: stray reads, wrong `fact not found` refusals, and answers taken from the wrong library."

Four options were weighed. **A**, one agent for every library, reads every router it judges relevant, which costs more as libraries are added, and its main session rewrote the answer and dropped the quotes and `Sources:` line: "only 3 or 4 of 24 delivered answers kept them." **B**, a triage subagent, was not built: a subagent cannot start other subagents, so it would add an agent hop and its startup tokens to every question. **C**, a triage desk in the main session with specialists, was chosen and measured. **D**, a graph or embedding router, was not tried: every failure measured was between two libraries that both cover the topic, and "A better router cannot separate those; only the libraries' own scope statements can."

On the 24-case subsample, one trial per case:

| | Single agent, 6 libraries | `kb-ask`, 6 libraries | Single agent, 48 libraries | `kb-ask`, 48 libraries |
|---|---|---|---|---|
| Routing | 23/24 | 23/24 (adjusted) | 19/24 | 20/24 |
| Delivered with quotes and sources | 4/24 | 23/24 | 3/24 | 21/24 |
| Answered from the wrong library | 0 | 0 | 0 | 2 |
| Specialist tokens/question | 19.0k | 7.1k | 25.5k | 10.4k |
| Cost/question | $0.161 | $0.137 | $0.196 | $0.193 |

Against the issue's targets: at 6 libraries, no accuracy loss and no more than 1.3× the tokens, met (total tokens 0.77×); at 48 libraries, fewer tokens than the single agent, met (0.89×), with cost per question about equal "because the catalog sits in the main session's context". Not met: routing of at least 95% at 48 libraries, where "`kb-ask` scored 83% and today's agent 79%"; and no answers from the wrong library, because a decoy library answered two questions "because it holds devil's advocacy and pre-mortem material too."

It was adopted anyway, "for what it measurably improves": the specialist's quotes and `Sources:` line reach the person 21 to 23 times in 24, against 3 or 4; specialists cost a third as much, and that share stays roughly flat as libraries are added; and routing is no worse. The remaining failures are between libraries that both cover the topic, "a library-scope problem", whose lever is the `Not covered here:` line, which the decoys lacked.

The stated costs: router topics in the catalog cut both ways, fixing a question whose topic was missing from a description but drawing another to a neighbor whose topic name matched; the catalog grows with the library count, and past roughly 15 libraries a sub-catalog split will be needed; the desk runs on whatever model the main session uses, and was measured on Sonnet only. A read into a library the desk then rejected still counts as a routing failure in `grade-routing`: "a stray read is still a read."

**Status: Proposed.** The decision is to be accepted, closing the no-orchestrator decision's reopen trigger, when `kb-ask` routes at least 95% at 48 libraries with no answer from the wrong library, on decoys that carry a `Not covered here:` line; and rejected "if neighbors that state their scope still draw questions they don't cover". Its delivery plan still lists that rerun and a measure of the entry point in normal use; whether either has happened is in [[knowledge-architecture-open-questions]].

Source: [[adr-0018-triage-and-specialists]]
Verified: 2026-11-09 · by: agent · method: doc-review

## A search lists files before lines, and stops when it stops paying

**Files first.** "A Grep across the whole library uses `output_mode: files_with_matches`, which names the matching files and nothing else." The agent then outlines or searches inside the one to three likeliest files. The reason: "A content search across a library can return more text than one result holds, and reading that overflow back costs more than reading the files would." Each search keeps to a few alternatives, because "a pattern with a dozen matches nearly everything, and tells you nothing."

**The stop rule.** "For each question, stop after **8 reads** of library files with no passage that answers, or **5 library-wide searches**." Both limits are per question, counted across every library you search: "three libraries do not get fifteen searches." A section read counts as a read; a router `INDEX.md` and a heading outline do not. The agent then answers `fact not found — check KB`, lists what it checked under `Looked at:`, and ends that line with `(search limit reached)`. Part of an answer found is still given, with what could not be found named.

A field test on three libraries of another deployment, the largest of 389K words, compared the rules with the agent before them: five questions, two runs each per arm. Library-wide content searches fell from 47 to 8, results spilled past the inline limit from 21 to 2, answers were unchanged and neither control regressed. Over the three questions the library did not answer, cost fell 17% in total, at two runs per arm; one of the three rose 42%, which the test judged within run-to-run variance. Its verdict on the stop rule: "safe, since it never touched a control, but weak as a cost cap. It fired once in six not-found runs, about 10 tool calls after its own limits, and one run passed the search limit without firing." A question spanning three libraries made 10 and 12 library-wide searches without reaching 5 in any one, which is why the limits were then made per question.

A visible tally was tried and dropped: having the agent write `read 3 of 8` or `search 2 of 5` before each call that counts. "In three Sonnet runs on knowledge-architecture, it never wrote one, in its text or its thinking. An instruction it doesn't follow is worse than none". Holding the limit reliably "may need a mechanism outside the prompt." The per-question wording was not field-tested when it was made.

Source: [[agent-kb-retrieve]] · [[issue-147-not-found-cost]] · [[pr-159-search-limits-per-question]]
Verified: 2026-11-09 · by: agent · method: doc-review
