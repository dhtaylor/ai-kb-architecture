---
name: issue-147-not-found-cost
description: Source stub — engine issue 147, the cost of a fact-not-found answer, and its field test of the files-first and stop rules.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-11-09
tags: [knowledge-architecture, source]
keywords: [fact not found, cost, files first, files_with_matches, stop rule, search limit, field test, spills, Not covered here]
---
# Source: ai-kb issue 147: the cost of `fact not found`, and the field test of its fix

- **Title:** ai-kb issue 147, "kb-retrieve: a `fact not found` costs ~10× more when no `Not covered here:` line names the topic, and nothing helps a librarian fill that line in"
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** 2026-11-07 (the field-test comment; corrected 2026-11-09)
- **Date ingested:** 2026-11-09
- **Artifact location:** `ai-kb#147`, the comment headed "Field test of PR #158", read via the hosting platform's command-line client on 2026-11-09. The text is editable, not archived here: it is a public record in the engine's tracker.

A field A/B of the files-first and stop rules on three libraries of another deployment, which the comment does not name. Folded from it: the search and spill counts, the not-found cost change, the stop rule's behavior and the verdict. Not folded: the questions, which describe that deployment's content, and the per-run figures.
