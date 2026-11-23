---
name: script-kb-libraries
description: Source stub — header of the engine's kb-libraries script, the registry reader and its library states.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-10-28
tags: [knowledge-architecture, source]
keywords: [kb-libraries, registry, ok, missing, not-a-library, name-mismatch, unsupported-contract]
---
# Source: kb-libraries (script header)

- **Title:** kb-libraries (script header)
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** undated in the artifact
- **Date ingested:** 2026-10-28
- **Artifact location:** `ai-kb:scripts/kb-libraries` (docstring), read at engine commit `c9dc309` on 2026-10-28. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.

Source for the states a registered library can be in and that only `ok` is served. Folded from it: the fifth state, `unsupported-contract`, and why it is not served. The four older states are folded from the registry decision.
- **Re-read:** on 2026-11-20 at engine commit `5433f08`, the usage line, defaults and exit codes. Nothing already folded changed.
- **Re-read:** on 2026-11-23 at tag `v0.5.0` (commit `3b60c08`), the `declared_terms` docstring, for retrieval-agent-behavior.md's neighbor-overlap term normalization. Unchanged. The code in `kbengine/library.py` (`normalize_term`) is narrower than this docstring: it drops a trailing `s` only from a term longer than 3 characters that does not end in `ss`.
- **Re-read:** on 2026-11-23 at engine `main` commit `5f6e073`, after #230: the `declared_terms` docstring now reads "a trailing `s` dropped from a term longer than 3 characters that does not end in `ss`", matching `normalize_term`. Folded into retrieval-agent-behavior.md, "Neighboring libraries are separated by a scope line, and registration warns of an overlap". Behavior unchanged since v0.5.0.
