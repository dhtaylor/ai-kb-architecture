---
name: engine-contributing-schedulable
description: Source stub — the engine's CONTRIBUTING.md: setting up a machine, the daily loop, troubleshooting, working on the engine, adding a library, and the section on commands that are safe to schedule.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-10-28
tags: [knowledge-architecture, source]
keywords: [schedulable, cron, systemd timer, exit codes, stripped environment, canary, regression case]
---
# Source: ai-kb CONTRIBUTING, Commands you can schedule

- **Title:** ai-kb CONTRIBUTING (whole file; first folded for its section Commands you can schedule)
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** undated in the artifact
- **Date ingested:** 2026-10-28
- **Artifact location:** `ai-kb:CONTRIBUTING.md`, section *Commands you can schedule*, read at engine commit `c9dc309` on 2026-10-28. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.
- **Re-read:** at engine commit `d16b78c` on 2026-10-28, with the engine's `main` at that commit: the section gained one paragraph, saying `kb-eval-sample` is not scheduled because it needs a person to start each agent run. The five-part property and what was folded from it are unchanged.

Defines the property a command must have to run unattended, and lists the schedulable form of each command with its exit codes. Folded from it: the property's five parts and how it is tested. Not folded: the table of exit codes per command, which is the engine's reference.
- **Re-read:** on 2026-11-20 at engine commit `5433f08`, the whole file, not only the section *Commands you can schedule*. Folded from the other sections, in [[using-ai-kb]]: setting up a machine, in each project, the daily loop, the troubleshooting list, the engine's own hook, the contributing rules and adding a library. Not folded: the curating rules, the hook check table and the sweep cadence. The five-part property and what was folded from the schedulable section are unchanged.
- **Re-read:** on 2026-11-24 at engine commit `303082a`, the check table and the hook and CI paragraph. Folded into [[using-ai-kb]]: `check-stamps` is retired (ADR-0022) and absent from the CI list, and the `check-golden` row names `expected_evidence`. The `kb.nudge` command and the schedule text are unchanged.
