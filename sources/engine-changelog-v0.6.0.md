---
name: engine-changelog-v0.6.0
description: Source stub — the engine's change notes for v0.6.0 (the changes after v0.5.0): evidence sets the lease, check-stamps retired, contract 2.7, coverage in the nudge, Attention.items, the kbengine modules.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-11-24
tags: [knowledge-architecture, source]
keywords: [changelog, v0.6.0, Unreleased, ADR-0022, unasserted_ttl, check-stamps, assertion coverage, Attention.items, kbengine.lease, kbengine.lint, kbengine.golden, kbengine.citations, kbengine.transcripts, kbengine.reports, factfiles.tree, kb-due --session, --summary-out, expected_evidence]
---
# Source: ai-kb CHANGELOG, section Unreleased (the changes after v0.5.0)

- **Title:** ai-kb CHANGELOG, section Unreleased (what ships as v0.6.0)
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** undated in the artifact; the section is headed `## Unreleased` and the engine's v0.5.0 is dated 2026-11-23
- **Date ingested:** 2026-11-24
- **Artifact location:** `ai-kb:CHANGELOG.md`, section `## Unreleased`, read at engine commit `303082a` on 2026-11-24. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record. To be re-pinned to tag `v0.6.0` after the release, as [[engine-changelog-v0.5.0]] was.

The engine's own account of what was built after v0.5.0. Folded from it: the evidence-set lease (#238), unasserted_ttl and contract 2.7 (#236), check-stamps retired (#240), coverage at the head of the nudge and report (#239), what retrieval states about evidence (#237), the lint's findings by section (#235) and its move into `kbengine.lint` (#249), `kbengine.lease` (#250), `factfiles.tree` (#251), the golden-set modules (#252), the one Attention policy and the schedule's move into `kb-due --session` (#253), the fence rule for links (#257), the plural-rule docstring (#230, already folded) and `kb-bootstrap --check` (#229). Not folded: byte-for-byte output-equivalence claims beyond what the entry states as changed behavior, and test-suite changes.
