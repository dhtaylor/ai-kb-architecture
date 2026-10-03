---
name: issue-9-kb-capture-scan
description: Source stub — engine issue 9, kb-capture wrote secrets and identifiers into notes unflagged, and the owner's decision to refuse.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-10-02
tags: [knowledge-architecture, source]
keywords: [issue, kb-capture, working tree, retrieval, refuse not warn, decision, untracked]
---
# Source: engine issue 9 and its decision comment, kb-capture and the working tree

- **Title:** kb-capture writes secrets and PII into episodic notes unflagged until commit
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** 2026-10-02 (closed by pull request 10)
- **Date ingested:** 2026-10-02
- **Artifact location:** `ai-kb#9` (the issue description and its decision comment), read via the hosting platform's command-line client on 2026-10-02. The text is editable, not archived here: it is a public record in the engine repository.

The defect, its reasoning, and the owner's decision comment (refuse, not warn). Folded: why commit-time is too late, the decision, and that the worktree scan now includes untracked files. The issue body proposed warning rather than refusing and flagged that as the owner's open question; the decision comment answers it, and only the decision is folded. Not folded: the proposed `--redact` option, which the decision does not adopt.
