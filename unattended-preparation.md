---
name: unattended-preparation
description: Why the engine may prepare knowledge work unattended but never decide it — the split of kb-distill and kb-audit at their review gate into review packets, why a headless run cannot write, the weekly report of decisions, the opt-in schedule, what makes a command safe to schedule, the verified backup, what is never automated, and what the design costs.
memory_type: semantic
domain: knowledge-architecture
scope: general
verified: 2026-10-28
metadata:
  type: fact
  node_type: memory
  created: 2026-10-28
tags: [knowledge-architecture, automation, unattended, governance]
keywords: [unattended, preparation, review packet, kb-prepare, kb-apply, kb-report, kb-schedule, kb-backup, headless, claude -p, read-only tools, STALE, weekly report, schedulable, systemd timer, cron, dollar cap, never automated, attended, ADR-0017, ADR-0013, stripped environment, canary]
---
# Unattended preparation, attended decisions

The standing rule is to automate detection and gate mutation. This file holds how far that was pushed: a model may do the legwork before a human decision on a schedule, and no fact is edited without a person.

## Preparing is separable from deciding

The first decision on steady state ran nothing unattended. It held that `kb-audit` and `kb-distill` "both need an agent and a human decision", so a person ran the mechanical checks by hand and the session-start nudge said what was due. Its own consequence named the weak point: nothing happens unless a session starts. That point arrived in normal use. Keeping a library current cost a session, a command per library per check, and a wait before there was anything to decide. A nudge reports work, so it grows when nobody does the work and becomes a list of chores rather than a list of decisions.

The change was to see that the phrase bundled two things. The legwork (finding what is due, extracting what a note says, building the side-by-side table a reviewer reads) needs an agent and not a person. Only the decision needs a person. Fact edits stay attended, and that is the constraint every later choice keeps.

Four options were weighed. The first, chosen: the engine ships an opt-in schedule that runs the preparation half and a weekly report of what needs deciding. The second: make commands safe to schedule and leave scheduling to each consumer. It leaves every consumer writing a wrapper, which is where the failures were. The third: a cloud routine. It runs while the machine is off but needs credentials for private library repositories, which is why it was declined before, and nothing changed that. The fourth: keep the first decision, whose cost grows with the number of libraries.

What stands from the first decision: the nudge at session start, the routing of the queue, the offline Watcher and the capture guide.

Source: [[adr-0017-unattended-preparation-attended-decisions]] · [[adr-0013-steady-state-surfaced-not-scheduled]]
Verified: 2026-10-28 · by: agent · method: doc-review

## A curation skill splits at its existing review gate

Each of the two skills already stopped for review at a gate. Everything before the gate is prepare and may run unattended. Everything after is decide and is always attended.

- **Prepare** covers `kb-distill`'s extraction and its fact and source table, and `kb-audit`'s findings. The result is a review packet, written outside every library and never committed.
- **Decide:** a person reads the packet and approves, edits or rejects each item. Only then does the skill write a fact file, run the checks and commit.

Only `kb-distill` and `kb-audit` get a prepare step. `kb-update-domain` folds external material and `kb-organize-domain` reshapes structure, and both stay fully attended. A packet records the library commit it was drafted against. `kb-apply` marks it STALE when the library has commits since, and a stale packet is still usable, because the skill re-validates each item against the current files before showing it. `kb-apply` itself never writes to a library: it lists packets, shows one, and closes one after the skill has acted on it.

`kb-prepare` runs a model only when there is work: notes awaiting distillation, or a sweep that is due. A packet already waiting for the same library and skill is not drafted again. `kb-audit`'s first step runs the mechanical checks, which the model cannot do without a shell, so the script runs them and gives the model their results.

Source: [[adr-0017-unattended-preparation-attended-decisions]] · [[script-kb-prepare]] · [[script-kb-apply]]
Verified: 2026-10-28 · by: agent · method: doc-review

## A headless run cannot write, and that is enforcement rather than instruction

The preparation run is `claude -p` given `Read`, `Grep` and `Glob` and nothing else: no MCP servers, no session persistence, and a dollar cap (default 1.00). It has no write, shell or network tool, so it cannot change a library or send what it reads anywhere. The reason this is stated as enforcement: a skill's `allowed-tools:` only pre-approves a tool, and the `--tools` list is what removes the others.

Two observations back it, each a single run reported by the change's author and not repeated here. In the decision of 2026-10-23, a `-p` run with that list, told to overwrite a file by any means, reported it had no tool to do so and the file was unchanged (Claude Code 2.1.309). In the change notes for the preparation script, merged the same day, one real run of `kb-prepare` on a scratch clone left the library unchanged, did not act on an instruction planted in the note, and cost $0.55.

The wrapper, not the model, writes the packet's header (the skill, the library, the commit the library was at, the date). The model's text is the body, and it passes through the redaction filter first, because a packet quotes source text (see [[secret-governance]]). Episodic notes can carry text that reads as instructions. A run that can only read, whose output a person reviews, can produce a bad packet and cannot act on one.

The limit, stated by the decision: the read-only tool list bounds what the model can do, not what it concludes, so a packet can be wrong in ways that read as right. The packet shows each item's source excerpt beside it so the reviewer can check rather than trust. The review is the control.

Source: [[adr-0017-unattended-preparation-attended-decisions]] · [[script-kb-prepare]] · [[engine-changelog-unreleased]]
Verified: 2026-10-28 · by: agent · method: doc-review
Recheck: grep -qF 'TOOLS = "Read,Grep,Glob"' "$KB_ENGINE_ROOT/scripts/kb-prepare"

## The weekly report lists decisions and holds no knowledge

`kb-report` writes one file a week, outside every library, listing per library only what a person has to decide:

- a review packet waiting, and how to act on it;
- each open queue item, by category and location;
- rechecks that are due but marked `Recheck (attended):`, which only a person can run;
- a sweep that is due, or notes awaiting distillation, that no packet covers yet;
- a library behind the contract or on one the engine cannot serve.

A library with nothing to decide gets no section, and a week with nothing to decide gets one line. The file is written either way, so a quiet week can be told from a week the job did not run. It never holds a fact's, a source's or a packet's text: paths, counts, headings and ids only. `kb-due --nudge` names the latest report while it is under eight days old and lists decisions, so the person who opens a session is pointed at it.

Source: [[adr-0017-unattended-preparation-attended-decisions]] · [[script-kb-report]] · [[script-kb-due]]
Verified: 2026-10-28 · by: agent · method: doc-review

## The schedule is opt-in and edits no fact

Nothing runs unless the user installs it with `kb-schedule install`, and `kb-schedule remove` leaves no trace. The job is a systemd user timer on Monday at 07:00 that catches up a missed week at the next start, or a crontab line where there is no systemd. For every registered library it runs, in order:

1. `kb-verify --yes --unattended` and `kb-sources --yes`, which only ever queue. Installing the schedule is the consent `--yes` asks for.
2. With `--prepare`, chosen at install, `kb-prepare distill` and `kb-prepare audit` under the dollar cap. It is a second opt-in because it is a model run that costs money.

Then, once, it runs `check-xlinks --installed`, `kb-watch` on the allowlist chosen at install, and `kb-report`. It holds a lock, so runs never overlap, and its log records each step's exit code and nothing a step printed. A consumer who installs nothing sees no change.

What it costs, as the decision states it. The job runs only while the machine is up: without systemd lingering a user timer runs only while the user is logged in, and a WSL distribution stops when idle, so catch-up depends on the timer's `Persistent=true` and on the machine being used. Preparation spends money every week whether or not anyone reads the report, which the cap bounds. And the engine gains moving parts, a scheduler, a packet format and an apply step, each with failure modes and tests of its own.

Source: [[adr-0017-unattended-preparation-attended-decisions]] · [[script-kb-schedule]]
Verified: 2026-10-28 · by: agent · method: doc-review

## A command is schedulable only if it behaves the same from nowhere

A command is schedulable when it is safe to run from cron, a systemd timer or `kb-schedule`:

- it never prompts;
- it works from any directory with no shell startup files, tested as `env -i HOME=… PATH=/usr/bin:/bin` from `/`;
- exit 0 means clean, 1 means findings, 2 means it could not run;
- it never prints a fact's or a source's text or a secret-check hit, only paths, headings, URLs, counts and ids;
- it is idempotent, and a queue write replaces the file in one rename, so it can run beside a session working in the same library.

Each schedulable command is a regression case run from `/` with a stripped environment, against fixtures that carry a canary in the text a careless command would echo. The reason the property exists: one consumer's cron wrapper reported FAIL for every backup for a week because cron started it in `$HOME`. The failures came from the wrappers, which is why the engine owns the property.

Meeting it changed some commands. `kb-watch` now exits 1 when a source drifted or was unreachable, where it exited 0 before, so a wrapper that read 0 as "ran" must accept 1. `kb-verify` shows a failed assertion's output, and `kb-watch` shows old and new values, only at a terminal or with `--dry-run`. `kb-due` always exits 0, because it is a reminder and the session hook reads any other exit as a failure.

Source: [[engine-contributing-schedulable]] · [[adr-0017-unattended-preparation-attended-decisions]] · [[script-kb-watch]] · [[engine-changelog-unreleased]]
Verified: 2026-10-28 · by: agent · method: doc-review

## A backup is verified by restoring it

Each library is its own repository, and one with no permitted remote has no second copy. `kb-backup DEST` makes one: a dated run directory holding a git bundle of the engine and of every registered library, the refs each held, and a checksum file.

A bundle is verified by restoring it into a scratch repository and comparing the restored refs with the ones recorded when it was made. `git bundle verify` is not used, because it reads only a bundle's header, so a truncated bundle passes it. `--restore-test` also clones each bundle and runs `git fsck --full`. A destination inside a backed-up repository is refused, since the destination holds everything the libraries hold and is as sensitive as they are. `--keep N` prunes only directories the command wrote. A library with no commits is skipped with a note, because git cannot bundle an empty repository and that is not a backup fault.

Source: [[script-kb-backup]] · [[adr-0017-unattended-preparation-attended-decisions]]
Verified: 2026-10-28 · by: agent · method: doc-review

## What stays attended whatever the schedule does

Resolving a `CONFLICTED` section, deleting knowledge, promoting scope, writing a human stamp, applying the `owner-approved` label, and any fact edit without a person's approval are never automated. `check-blast` still gates the first three. The schedule stores no credentials.

The decision names its own review triggers: a packet is applied that a reviewer later finds wrong, the weekly report goes unread for a month, or a cloud schedule can run without stored credentials.

Source: [[adr-0017-unattended-preparation-attended-decisions]]
Verified: 2026-10-28 · by: agent · method: doc-review
