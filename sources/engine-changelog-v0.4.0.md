---
name: engine-changelog-v0.4.0
description: Source stub — the engine's CHANGELOG section for the changes after v0.3.0, released as v0.4.0.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-11-09
tags: [knowledge-architecture, source]
keywords: [CHANGELOG, v0.4.0, release notes, read-only, links_to, contract 2.6, dependency mismatch]
---
# Source: ai-kb CHANGELOG, section Unreleased (v0.4.0)

- **Title:** ai-kb CHANGELOG, section Unreleased (to be released as v0.4.0)
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** undated in the artifact; the section lists the changes on `main` after v0.3.0
- **Date ingested:** 2026-11-09
- **Artifact location:** `ai-kb:CHANGELOG.md`, section `## Unreleased`, read at engine commit `5fb833a` on 2026-11-09. To be re-pinned to tag `v0.4.0` after the release, as the v0.3.0 stub was. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.

The engine's own account of what was built after v0.3.0. Cited where a decision record says what was decided and the change notes say what was delivered: the read-only role, contract 2.6 and `--ref` pinning (ADR-0020 steps 1 to 3). Release notes are not folded.

- **Re-read:** on 2026-11-11 at engine `main` commit `dfa93f9` (PR #179), where the Unreleased section gained the entry for `--ref` pinning (ADR-0020 step 3, #148) and the ADR-0020 umbrella entry still read "Delivered in steps: the read-only role, then `links_to` (contract 2.6), then `--ref` pinning." Folded from that entry: how `--ref` is used and what the registry and `kb-library list` show. Nothing folded earlier changed.
- **Release heading:** at engine release-branch commit `6c2197e` (`release-v0.4.0`, local, unmerged when read) the section is headed `## v0.4.0 — 2026-11-11`, states that the engine serves contracts 2.0 to 2.6, and the umbrella entry reads "Delivered in three steps, all in this release". That is the source for "all in v0.4.0". The entries themselves are unchanged from `dfa93f9`. To be re-pinned to tag `v0.4.0` after the release, as the v0.3.0 stub was.
