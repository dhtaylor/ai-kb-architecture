---
name: agent-kb-retrieve
description: Source stub — the engine's kb-retrieve agent definition, the retrieval contract an agent is held to.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-10-16
tags: [knowledge-architecture, source]
keywords: [kb-retrieve, engine root unknown, library list unknown, quote, word for word]
---
# Source: the engine's kb-retrieve agent definition, the retrieval contract an agent is held to

- **Title:** kb-retrieve (agent definition)
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** undated in the artifact
- **Date ingested:** 2026-10-16
- **Artifact location:** `ai-kb:plugins/knowledge-tools/agents/kb-retrieve.md`, read at engine commit `c9dc309` on 2026-10-28. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.

Source for two as-built rules: the agent treats the libraries its context lists as the complete set and never searches a folder for others, and it quotes what an answer rests on word for word. The rest of the contract lives in `ai-kb:CONVENTIONS.md` and is not restated. Re-read 2026-10-28 for rule 1 (the scope line, section-targeted reads, searches scoped to the routed library) and rule 3 (what the knowledge base is, and that the engine repository is not part of it). Re-read at engine commit `5fb833a` on 2026-11-09 for rule 1's files-first search, rule 4's stop rule and the specialist mode; the rules folded earlier still read as folded.
- **Re-read:** at engine commit `5433f08` on 2026-11-20, for the agent's description, `tier: retrieval`, `tools: Read, Grep, Glob` and `model: sonnet`, folded into skills-agents-and-commands.md; the retrieval rules were not re-folded.
- **Re-read:** on 2026-11-24 at engine commit `303082a`, rule 6 and the answer format. Folded into [[retrieval-agent-behavior]]: the agent states what backs each cited fact ("carries a check, last passed <date>", "someone's word, <date>", "unstamped"), flags the last two before the answer and prints an `Evidence:` line where it printed `Verified:`. Rules 1 to 5 were not re-folded.
