---
name: script-kb-session-start
description: Source stub — header of the engine's kb-session-start script, the session and subagent hook.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-10-28
tags: [knowledge-architecture, source]
keywords: [kb-session-start, SessionStart, SubagentStart, systemMessage, additionalContext, nudge]
---
# Source: kb-session-start (script header and body comments)

- **Title:** kb-session-start (script header and body comments)
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** undated in the artifact
- **Date ingested:** 2026-10-28
- **Artifact location:** `ai-kb:scripts/kb-session-start` (header and the comments in its body), read at engine commit `c9dc309` on 2026-10-28. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.

Source for what the hook injects at session start and for a retrieval subagent, and how the nudge reaches the person. Folded from it: the subagent form and why it exists, and the one-line message. Not folded: the shape of the hook's output.
