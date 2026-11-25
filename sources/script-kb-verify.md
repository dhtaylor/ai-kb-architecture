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
- **Re-read:** on 2026-11-12 at tag `v0.4.0` (commit `a318c28`; the file's blob is `9ce152e`). Folded from it: a library whose `INDEX.md` declares no `verifier_budget:` gets a budget of 5, and `--budget N` overrides the declared one.

Corroboration for the Verifier as built: reports and applies only when told, a failure is never applied, commands are approved per run, and a failure is written to the owning library's queue. Used for built behaviour only. Re-read 2026-10-28 for the unattended form: `--unattended` skips `Recheck (attended):` assertions, runs the rest with an empty `HOME` and only `PATH`, `KB_ENGINE_ROOT` and proxy settings, and refuses `--apply`.
- **Re-read:** on 2026-11-20 at engine commit `5433f08`, the usage line, defaults and exit codes. Nothing already folded changed.
- **Re-read:** on 2026-11-23 at tag `v0.5.0` (commit `3b60c08`): the queue is now written through `kbengine.queue`, as kb-watch does (#202, engine commit `d98fa91`, 2026-11-16), superseding the import of kb-watch's queue primitives. The 2026-11-20 re-read missed this. Folded into agent-lifecycle-roles.md, "The Watcher as built".
- **Re-read:** on 2026-11-24 at engine commit `303082a`, the docstring and the usage text. Folded from it: selection by the lease the evidence sets (ADR-0022), made by `kbengine.lease`; each command names its lease and the lint finding behind a short one; the assertion lint is `kbengine.lint`. The usage line, exit codes and the three rules are unchanged.
- **Re-read:** on 2026-11-24 at engine commit `a9d98b0` (main after #264), `scripts/kb-verify` after #264: it writes a record's `file:` and `stamp:` through `lease.record_file` and `lease.stamp_text`, which the lease reader shares. Folded into [[script-reference-tools]].
