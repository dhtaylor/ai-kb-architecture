---
name: knowledge-agent-architecture-combination-plan
description: Source stub — the advisory reference-design proposal for combining a governed knowledge base with a thin-agent fleet, whose decisions are recorded separately as ADRs.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-09-19
tags: [meta, proposal, source]
keywords: [combination plan, thin agent, governed KB, reference design, proposal]
---
# Source: Combining the Governed Knowledge Base with a Thin-Agent Fleet — Implementation Plan

- **Title:** Combining the Governed Knowledge Base with a Thin-Agent Fleet — Implementation Plan
- **Author:** Dandy Taylor (with Q)
- **Date:** 2026-09-18
- **Date ingested:** 2026-09-19
- **Artifact location:** `documents/knowledge-agent-architecture-combination-plan.md` (archived in this repository — evidence travels with the domain it was folded from)

An advisory reference-design proposal, hardened by a five-lens adversarial review, for turning
this knowledge base from a passive reference library into an actionable system: a thin agent
layer that retrieves from the KB and acts, plus a lifecycle that keeps the knowledge current.
Written to be reusable across projects. Its concrete *decisions* are recorded as
`ai-kb:documents/decisions/0001-kb-root-resolution.md`, `ai-kb:documents/decisions/0002-governance-without-enforcement.md`,
`ai-kb:documents/decisions/0003-scoping-topology.md`, `ai-kb:documents/decisions/0004-scope-attribute.md` and
`ai-kb:documents/decisions/0005-currency-cadence.md` — this stub and the facts citing it cover only the surrounding
reference design (mechanisms, roles, thresholds, failure modes) that is true independent of
which option this workspace chose. Its §6 (phased implementation plan) and §8 (open
decisions/next actions) are project management, not durable knowledge, and were not folded.

**Superseded 2026-10-02** by [[knowledge-agent-architecture-combination-plan-revised]] (section 1 rewritten, section 9 added). This stub and its archive are kept as evidence of what was folded first.
