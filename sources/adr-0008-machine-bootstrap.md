---
name: adr-0008-machine-bootstrap
description: Source stub — engine ADR-0008, one command registers the engine on a machine and git config carries it to hooks, after a hook silently skipped its checks outside interactive shells.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-10-02
tags: [knowledge-architecture, source]
keywords: [bootstrap, machine setup, git config, hook, non-interactive shell, drift, ADR]
---
# Source: engine ADR-0008, one command registers the engine on a machine, and git config carries it to hooks

- **Title:** ADR-0008: One command registers the engine on a machine, and git config carries it to hooks
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** 2026-09-22
- **Date ingested:** 2026-10-02
- **Artifact location:** `ai-kb:documents/decisions/0008-machine-bootstrap.md`, read at engine commit `35f0e95` on 2026-10-02. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.

The engine's decision record for machine setup. Folded from it: that a hook fired outside an interactive shell skipped its checks while exiting 0, that git's global configuration is the channel hooks now use, its test of a commit from a non-interactive shell, and its stated consequences (an unset setting makes hooks skip rather than fail; a GUI client or IDE was reasoned about, not tested). Not folded: the bootstrap command's options, the four places it writes, and the shell-startup behaviour itself, which belongs in the claude-code-runtime library and is linked from there.
- **Re-read:** on 2026-11-20 at engine commit `5433f08`, the whole record. Folded in [[using-ai-kb]] beside what the stub already lists: the options considered are still not folded; the hook script's move into the engine (`scripts/kb-session-start`), the retirement of `KB_GENERAL_ROOT`, the two-step team instruction (`kb-bootstrap` once per machine, `kb-init` once per clone), `kb-init` wiring an already-initialised project, and the tests recorded on a throwaway `HOME`.
