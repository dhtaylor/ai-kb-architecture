---
name: knowledge-agent-architecture-combination-plan-revised
description: Source stub — the combination plan as revised from build findings (section 1 rewritten 2026-09-19, section 9 findings from implementation), which supersedes the 2026-09-18 version.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-10-02
tags: [knowledge-architecture, proposal, source]
keywords: [combination plan, revised, target architecture, three repository kinds, findings from implementation, as built]
---
# Source: Combining the Governed Knowledge Base with a Thin-Agent Fleet — Implementation Plan (revised)

- **Title:** Combining the Governed Knowledge Base with a Thin-Agent Fleet — Implementation Plan
- **Author:** [redacted: author] (with Q)
- **Date:** header reads "2026-09-18 · Revised: 2026-09-19"; later sections carry their own dates through 2026-10-01 (section 9.19)
- **Date ingested:** 2026-10-02
- **Artifact location:** `documents/knowledge-agent-architecture-combination-plan-revised.md` (archived in this repository). The original lives in the engine at `ai-kb:documents/knowledge-agent-architecture-combination-plan.md`.

The revised plan. It **supersedes** [[knowledge-agent-architecture-combination-plan]] (the 2026-09-18
version, kept as evidence and not rewritten). Section 1 (target architecture) was rewritten on
2026-09-19 after building showed one tree violated the plan's own thesis; section 9 records what
implementation proved. Folded from it: section 0 (re-verified, identical to the original); section 1; the parts of 5A and 6
that bear on the lifecycle roles (including the Phase 4 and Phase 5 entries of 6); and from section 9:
9.1, 9.5 (the two general lessons only; the nine individual contract defects are rationale already
encoded in the contract), 9.6, 9.8, 9.9, 9.11 (the retired-versus-dropped line only), 9.12, 9.13, 9.14,
9.15, 9.16 (as a retrieval-eval lesson), 9.17 (the eval lessons only), 9.18 and 9.19. Not
folded: 9.2 and 9.3 (Claude Code runtime behaviour, which belongs in the claude-code-runtime library),
9.4 (cross-repository citation: the rule is in the contract, section 6), 9.7 (fold rules confirmed
under test), 9.10 (on sequence), the skill-specific parts of 9.17 and the runtime's tool-scoping
behaviour in 9.18 (an open question here), and sections 2, 3, 4, 5, 7 and 8 (the library cites 2 and 5.4
only from the original plan's stub). The 8-of-roughly-30 ratio in 9.13 is also not folded.

**Redaction:** the author line of the archived copy originally named a person. The name was replaced
with "[redacted: author]" because this library is public and records no personal names. Nothing else
in the archive was altered. The owner may reverse this; the original is in the engine.
