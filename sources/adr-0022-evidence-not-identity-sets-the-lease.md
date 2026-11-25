---
name: adr-0022-evidence-not-identity-sets-the-lease
description: Source stub — engine ADR-0022, the evidence behind a stamp, not who wrote it, sets its lease; agent_ttl becomes unasserted_ttl at contract 2.7; check-stamps is retired.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-11-24
tags: [knowledge-architecture, source]
keywords: [ADR-0022, asserted stamp, unasserted stamp, lease, evidence tier, unasserted_ttl, agent_ttl, check-stamps, assertion coverage, Arbiter, expected_evidence, kb-retrieve]
---
# Source: ADR-0022: Evidence, not identity, sets a stamp's lease

- **Title:** ADR-0022: Evidence, not identity, sets a stamp's lease
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** 2026-11-23
- **Date ingested:** 2026-11-24
- **Artifact location:** `ai-kb:documents/decisions/0022-evidence-not-identity-sets-the-lease.md` at engine commit `303082a`. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record. Status Accepted; target v0.6.0.
- **Re-read:** on 2026-11-24 at engine commit `a9d98b0` (main after #264), the new implementation bullet, "Passing" means no open failed-recheck record raised against the current stamp (2026-11-24, #264). Folded into [[agent-lifecycle-roles]] and [[contract-provenance-and-currency]].

The engine's decision record for what sets a stamp's lease. Folded from it: the problem, the options weighed, the nine decisions, the consequences and the section on what changed at implementation. Not folded: the decider, the figures for the six libraries registered on the engine's machine on 2026-11-23 beyond the headline.
