---
name: adr-0021-engine-state-at-fixed-paths
description: Source stub — engine ADR-0021, the engine keeps its state and config at fixed paths and does not read XDG_STATE_HOME or XDG_CONFIG_HOME.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-11-20
tags: [knowledge-architecture, source]
keywords: [XDG_STATE_HOME, XDG_CONFIG_HOME, fixed paths, schedule.conf, KB_PACKETS, KB_REPORTS, CLAUDE_CONFIG_DIR, kbengine.env, cron, systemd, silent failure]
---
# Source: ADR-0021: The engine keeps its state and config at fixed paths

- **Title:** ADR-0021: The engine keeps its state and config at fixed paths
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** 2026-11-17
- **Date ingested:** 2026-11-20
- **Artifact location:** `ai-kb:documents/decisions/0021-engine-state-at-fixed-paths.md` at commit `5433f08`. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.

The engine's decision record for where the engine's own files live. Folded from it: the problem and the failure it caused, the options weighed, the paths, the two overrides, the one module that reads the environment, `CLAUDE_CONFIG_DIR`, what is out of scope, and the consequences for a person who had set either variable. Not folded: the decider, the issue link and the review trigger.
