---
name: conventions-contract-rules
description: Source stub — the engine contract, CONVENTIONS.md sections 1 to 13 — scope tiers, layout, router, granularity, slugs, provenance, currency, contradictions, promotion, the retrieval contract, frontmatter, naming and the list of checks.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-11-20
tags: [knowledge-architecture, source]
keywords: [contract, CONVENTIONS, scope tiers, layout, router, slugs, provenance, currency, CONFLICTED, UNCORROBORATED, promotion, retrieval contract, golden set, frontmatter, naming, verifying a domain]
---
# Source: ai-kb CONVENTIONS sections 1 to 13

- **Title:** ai-kb CONVENTIONS sections 1 to 13
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** undated in the artifact (contract version 2.6; its frontmatter records created 2026-07-08, revised 2026-10-28)
- **Date ingested:** 2026-11-20
- **Artifact location:** `ai-kb:CONVENTIONS.md`, sections 1 to 13, read at engine commit `5433f08` on 2026-11-20. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.

The operative contract every library conforms to. Folded from it: sections 1 to 13, as [[contract-layout-and-naming]] (1 to 5, 11, 12), [[contract-provenance-and-currency]] (6 to 9) and [[contract-retrieval-and-verification]] (10, 13). Not folded: the opening note on skill renames, the paragraph on three decisions between version 1 and the plan, and the rationale paragraphs that only repeat a decision record. Section 14 (contract versions) is folded from [[conventions-contract-versions]] and is in [[releases-and-contract-versioning]]. This stub is separate from [[conventions-contract-versions]], which is titled and keyworded for sections 3, 7 and 14 and carries the re-read history for them.
- **Re-read:** on 2026-11-23 at tag `v0.5.0` (commit `3b60c08`), section 7: an unmarked assertion that needs a credential fails rather than reaching one. Now cited by agent-lifecycle-roles.md, "The Verifier as built", which states it.
- **Re-read:** on 2026-11-24 at engine commit `303082a`, sections 3, 7, 10 and 13 (contract version 2.7): section 3 names `unasserted_ttl:` for `agent_ttl:`; section 7 replaces the trust tiers with evidence tiers, retires `check-stamps` in the `by:` paragraph and gates a high-blast-radius fact by consequence; section 10 adds the optional `expected_evidence:` golden field; section 13 drops the stamp-honesty row. Folded into [[contract-provenance-and-currency]], [[contract-layout-and-naming]] and [[contract-retrieval-and-verification]]. Section 13 still reads "All twenty-eight are built." above a table that now lists twenty-seven rows.
