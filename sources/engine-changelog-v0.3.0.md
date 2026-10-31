---
name: engine-changelog-v0.3.0
description: Source stub — the v0.3.0 section of the engine's CHANGELOG, the engine changes made after its first release.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-10-28
tags: [knowledge-architecture, source]
keywords: [CHANGELOG, v0.3.0, release notes, kb-library overlap, run-checks, grade-routing, contract 2.3, kb-sources, redact]
---
# Source: ai-kb CHANGELOG, section v0.3.0

- **Title:** ai-kb CHANGELOG, section v0.3.0
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** 2026-10-31, the release date in the section's heading (it lists the changes on main after v0.2.0)
- **Date ingested:** 2026-10-28
- **Artifact location:** `ai-kb:CHANGELOG.md` at tag `v0.3.0` (commit `6a779f3`), section `## v0.3.0 — 2026-10-31`. First read, as section `## Unreleased`, at engine commit `c9dc309` on 2026-10-28. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.
- **Re-read:** at engine commit `d16b78c` on 2026-10-28, with the engine's `main` at that commit: the section is still headed Unreleased. Since `c9dc309` it gained the entry that the library CI template clones a release tag, a note on systemd lingering and rewordings of the contract version and `kb-init` entries. Nothing folded from it changed.
- **Re-pinned:** to tag `v0.3.0` on 2026-10-31, after the release. `git diff d16b78c v0.3.0 -- CHANGELOG.md` changes only the section's heading (`## Unreleased` to `## v0.3.0 — 2026-10-31`) and its two-line introduction, which now states the contract range served, 2.0 to 2.3; every entry is byte-identical. No fact cites the heading or the introduction, so the facts that cite this stub keep their 2026-10-28 stamps: their other sources were not re-read here, and a new stamp would claim they were.

The engine's own account of what changed on `main` after its first release, with the issue numbers. Used where the engine's change notes are the only statement of a fix (the overlap warning, the test suite's git environment, the grader's attribution rules, the retry of a refused request). Where a script's own header states the rule, the script stub is cited as well. Release notes are not folded.
