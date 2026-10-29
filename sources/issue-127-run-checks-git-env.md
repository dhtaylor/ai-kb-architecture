---
name: issue-127-run-checks-git-env
description: Source stub — engine issue 127, the regression suite aimed its fixtures at the repository being committed to.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-10-28
tags: [knowledge-architecture, source]
keywords: [run-checks, linked worktree, core.bare, core.hooksPath, staged deletions, GIT_DIR]
---
# Source: ai-kb issue 127: tests/run-checks inherits git's repository environment from the hook

- **Title:** ai-kb issue 127: tests/run-checks inherits git's repository environment from the hook
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** 2026-10-26 (observed)
- **Date ingested:** 2026-10-28
- **Artifact location:** `ai-kb#127` (the issue description), read via the hosting platform's command-line client on 2026-10-28. The text is editable, not archived here: it is a public record in the engine's tracker.

The engine's record of a defect found on 2026-10-26: what the suite did to a shared repository when run from a hook in a linked worktree, and the repair. Folded from it: the observed effects and the fix. Not folded: the regression case's design beyond its purpose.
