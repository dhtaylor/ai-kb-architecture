---
name: retrieval-agent-behavior
description: How the retrieval agent decides where to look and what counts as an answer's source — what the knowledge base is and why the engine repository is not part of it, why it reads the section that answers and not the whole file, why a search is scoped to the routed library, how it picks between neighboring libraries with the scope line, why registration warns of an overlap, and how a subagent is given the engine and library list.
memory_type: semantic
domain: knowledge-architecture
scope: general
verified: 2026-10-28
metadata:
  type: fact
  node_type: memory
  created: 2026-10-28
tags: [knowledge-architecture, retrieval, routing, kb-retrieve]
keywords: [kb-retrieve, rule 3, off-KB, section read, outline, Grep scope, search is not a router, Not covered here, scope line, neighboring libraries, overlap warning, kb-library add, keywords, tags, SubagentStart, kb-session-start, library list, unsupported-contract, routers]
---
# Retrieval agent behavior

The retrieval contract (`ai-kb:CONVENTIONS.md` §10) says what an answer must contain. This file holds the choices in how the agent gets there: where it may look, how much it reads, and how it chooses among libraries. Each is a response to a failure seen in use.

## The knowledge base is the registered libraries and a project's tree, and nothing else

The agent's third rule says what the knowledge base is: the libraries its context lists and a project's own `knowledge/` tree. The engine repository is not part of it. Its decision records, eval records, plans and scripts describe how the knowledge base works, not what it knows, and its contract tells the agent how to retrieve and never what to answer. A decision record that mentions the topic is not an answer. When the libraries are silent the answer is `fact not found`, whatever a file outside them says.

The stated reason: "An answer taken from `.git/config` may be right, but it tells nobody whether the knowledge base holds the fact, and a gap nobody sees is a gap nobody fills." Reading the source is the caller's job.

The rule was added after a sample found a retrieval agent answering a question the library cannot answer from an engine decision record, to which a library file only points. `grade-eval` failed the case while the routing grader passed it, because that grader did not count a read outside every registered library. The routing grader fails a case with an off-KB read, meaning a Read, or a content-mode Grep result, of a file in no registered library, with the contract itself exempt because the session context tells the agent to load it. A read in an unregistered library is off-KB too. After the change, three fresh agents asked the failing question each answered `fact not found`, named the engine record as the place the caller could check, and did not open it; three trials of one question show the rule is followed for the question that failed, not that the failure cannot recur.

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

Source: [[agent-kb-retrieve]] · [[script-kb-library]] · [[engine-changelog-unreleased]] · [[conventions-contract-versions]]
Verified: 2026-10-28 · by: agent · method: doc-review

## A retrieval subagent is given the engine and the library list by a hook

A subagent never sees the session-start context, and without the engine path a retrieval agent searched whatever other knowledge tree its workspace held. `kb-session-start --subagent` produces the same context as a SubagentStart event for `kb-retrieve`: the engine path, the registered libraries by absolute path, and each registered library that cannot be served, named with its state and path. The librarian's due-nudge is left out, because it is for the person's session and not for a retrieval. The agent treats the listed libraries as the complete set and answers `engine root unknown — not searched` or `library list unknown — not searched` when its context lacks them; why is in [[retrieval-eval-integrity]], and how libraries are registered is in [[design-thesis]].

A library listed as registered but not served is reported as unavailable, and never as `fact not found`, when the question needs it: the gap is a broken install and not missing knowledge. A library on an unsupported contract major is one such case; see [[releases-and-contract-versioning]].

Source: [[script-kb-session-start]] · [[agent-kb-retrieve]] · [[script-kb-libraries]]
Verified: 2026-10-28 · by: agent · method: doc-review
