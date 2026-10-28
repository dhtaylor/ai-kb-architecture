---
name: script-kb-verify
description: Source stub — header of the engine's kb-verify script, the Verifier as built.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-10-02
tags: [knowledge-architecture, source]
keywords: [kb-verify, Verifier, Recheck, queue, assertion]
---
# Source: header of the engine's kb-verify script, the Verifier as built

- **Title:** kb-verify (script header)
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** undated in the artifact
- **Date ingested:** 2026-10-02
- **Artifact location:** `ai-kb:scripts/kb-verify` (docstring), read at engine commit `c9dc309` on 2026-10-28. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.
- **Re-read:** at engine commit `720fcff` on 2026-10-16, for `agent_ttl` and the report of assertions that cannot fail.

Corroboration for the Verifier as built: reports and applies only when told, a failure is never applied, commands are approved per run, and a failure is written to the owning library's queue. Used for built behaviour only. Re-read 2026-10-28 for the unattended form: `--unattended` skips `Recheck (attended):` assertions, runs the rest with an empty `HOME` and only `PATH`, `KB_ENGINE_ROOT` and proxy settings, and refuses `--apply`.
