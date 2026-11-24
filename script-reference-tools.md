---
name: script-reference-tools
description: Reference for ai-kb's kb-* scripts and the kbengine package at engine commit 303082a — for each kb-* script, what it is for, its usage as written, its defaults, its exit codes and what it never does, and what the kbengine modules are; where the design is explained, it links the section rather than restating it.
memory_type: semantic
domain: knowledge-architecture
scope: general
verified: 2026-11-21
metadata:
  type: fact
  node_type: memory
  created: 2026-11-20
tags: [knowledge-architecture, scripts, reference, tools]
keywords: [script reference, exit codes, usage, defaults, kb-apply, kb-backup, kb-bootstrap, kb-capture, kb-catalog, kb-due, kb-eval-sample, kb-init, kb-libraries, kb-library, kb-owner, kb-prepare, kb-report, kb-schedule, kb-session-start, kb-sources, kb-telemetry, kb-verify, kb-watch, kbengine, --apply, --yes, --nudge, --quiet, --summary, --budget-usd, --restore-test, --unattended, --dry-run, --read-only, --ref, schedulable]
---
# Script reference: the kb-* tools and the kbengine package

One section per `kb-*` script in the engine's `scripts/` folder, read at engine commit `5433f08`, then the `kbengine` package. Each gives the purpose in the header's own words, the usage as written, defaults, exit codes and what the script never does. The design behind a script is in the design files linked from its section and is not repeated. The check scripts are in [[script-reference-checks]]. A line marked "Observed" was seen by running the script at that commit and is not in the header.

## kb-apply

Purpose: "the review packets waiting for a person's decision (ADR-0017, #99)." It lists packets, shows one, and closes one once the skill has acted on it.

Usage, as written:

```
kb-apply list [--out DIR]
kb-apply show <packet>
kb-apply done <packet> applied|rejected
```

- Defaults: packets live in `$KB_PACKETS`, or `~/.local/state/ai-kb/packets` (ADR-0021), as `<library>/<date>-<skill>.md`. A closed one moves to `<library>/done/`.
- Exit codes: "Exit codes for list: 0 nothing pending, 1 packets pending, 2 could not run. show and done: 0 or 2."
- Never: "This script never writes to a library." `list` prints paths, libraries and dates only, never a packet's text, so it is schedulable.
- A packet is STALE when its library has commits since the one in its `library_head:`. A stale packet is still usable.

Packets and the review gate: already covered, [[unattended-preparation]] § A curation skill splits at its existing review gate and § A closed packet is kept.

Source: [[script-kb-apply]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: t=$(mktemp -d); r=0; "$KB_ENGINE_ROOT/scripts/kb-apply" list --out "$t" >/dev/null 2>&1 || r=1; mkdir "$t/x"; printf -- '---\npacket: kb-audit\nlibrary: x\nstatus: pending\nlibrary_path: /none\nprepared: 2026-11-20\n---\nb\n' > "$t/x/2026-11-20-kb-audit.md"; "$KB_ENGINE_ROOT/scripts/kb-apply" list --out "$t" >/dev/null 2>&1; [ $? -eq 1 ] || r=1; rm -rf "$t"; exit $r

## kb-backup

Purpose: "bundle the engine and every registered library, verified by restore (#96)."

Usage, as written:

```
kb-backup DEST [--keep N] [--restore-test]
kb-backup verify RUN
```

- `--keep N`: after a run, keep the newest N runs under DEST and delete older ones. "Only directories this script wrote (named for their time, holding SHA256SUMS) are ever deleted."
- `--restore-test` also clones each bundle and runs `git fsck --full`. `verify <run>` re-checks a run made earlier, the same way.
- Output: "One line per repository: ok, skip or FAIL, and no file content, so it is schedulable."
- Exit codes: "Exit 0 all ok, 1 any FAIL, 2 could not run."
- Never: "Nothing is written into the engine or any library, and a destination inside one of them is refused." A library with no commits is skipped with a note. Destination and schedule are the user's choice.

Why a bundle is verified by restoring it: already covered, [[unattended-preparation]] § A backup is verified by restoring it.

Source: [[script-kb-backup]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: "$KB_ENGINE_ROOT/scripts/kb-backup" >/dev/null 2>&1; [ $? -eq 2 ] && grep -qF 'all ok, 1 any FAIL, 2 could not run.' "$KB_ENGINE_ROOT/scripts/kb-backup"

## kb-bootstrap

Purpose: "register this engine on this machine. Run once per machine, and again whenever the engine moves; it is safe to run any number of times."

Usage: `kb-bootstrap [--check | --apply] [--library-home DIR]`

- No flag is the same as `--check`: "report what is configured and what would change; change nothing." `--check` and `--dry-run` are explicit spellings of the default. `--apply` makes the changes.
- `--library-home DIR` is where `kb-library` clones new libraries (`kb.libraryHome` in the registry). "Recorded as <engine>/kb when never set; a home already set is kept unless this names another. KB_LIBRARY_HOME overrides it."
- It writes four places: `~/.claude/settings.json` (`KB_ENGINE_ROOT`, the marketplace and plugin, the SessionStart hook and the same hook on SubagentStart), global git config `kb.engineRoot`, a managed block in the shell rc (PATH), and `core.hooksPath` in installed libraries.
- Exit codes: "Exit 1 if anything would change" in the default report mode. Observed: an unknown option exits 2, as does a folder that does not look like an engine.
- `--check` exits 1 when `kb-libraries` fails, where it exited 0 (#229). With `scripts/kbengine/` missing, as in a partial copy of the engine, `kb-libraries` failed at import and `--check` printed the traceback and read as no drift. It now reports `FAILED kb-libraries`, does not print `(none registered)`, and says that `--apply` cannot fix it: restore the engine, then run it again.
- Never: existing settings "are merged, never replaced"; a file is backed up before its first change, into `~/.local/state/ai-kb/backups/` and never beside it. Projects are wired per clone by `kb-init`, not from here. A linked target is named every run and "nothing is refused".

Why the engine registers itself once per machine: already covered, [[design-thesis]] § A library is installed on a machine by being registered there and § The engine's own files are at fixed paths, whatever XDG says.

Source: [[script-kb-bootstrap]] · [[engine-changelog-v0.6.0]] (#229)
Verified: 2026-11-24 · by: agent · method: doc-review
Recheck: "$KB_ENGINE_ROOT/scripts/kb-bootstrap" --bogus >/dev/null 2>&1; [ $? -eq 2 ] && grep -qF 'change nothing. Exit 1 if anything would change.' "$KB_ENGINE_ROOT/scripts/kb-bootstrap"
Recheck: t=$(mktemp -d); r=0; mkdir -p "$t/e/scripts" "$t/e/.claude-plugin" "$t/h"; cp "$KB_ENGINE_ROOT/CONVENTIONS.md" "$t/e/"; cp "$KB_ENGINE_ROOT/.claude-plugin/marketplace.json" "$t/e/.claude-plugin/"; for s in kb-bootstrap kb-libraries kb-library kb-session-start; do cp "$KB_ENGINE_ROOT/scripts/$s" "$t/e/scripts/"; done; o=$(HOME="$t/h" "$t/e/scripts/kb-bootstrap" --check 2>&1); [ $? -eq 1 ] || r=1; printf '%s\n' "$o" | grep -qF 'FAILED   kb-libraries' || r=1; printf '%s\n' "$o" | grep -qF 'so this report is incomplete. --apply cannot fix that' || r=1; printf '%s\n' "$o" | grep -qF '(none registered)' && r=1; rm -rf "$t"; exit $r

## kb-capture

Purpose: "file an episodic note into a knowledge tree. No model required." "CAPTURE RECORDS, IT DOES NOT CLASSIFY."

Usage: `kb-capture [--title TITLE] [--into TREE] [--from FILE] [DIR]`

- `DIR` is anywhere inside a project with a knowledge tree (default: here). `--into` files into this tree instead (a library, or a project's `knowledge/`). `--from` takes the note body from FILE ("-" for stdin) instead of an editor. `--title` is one line and becomes the filename and description.
- Exit codes: the header states none. Observed: an unknown option or a missing `--title` exits 2; a refused or empty note exits 1.
- Never: no tier decision, domain choice, currency stamp or judgment about what counts as a fact. "ONE THING IT DOES REFUSE": a note holding a credential or a personal identifier. A refused draft is kept outside the tree and nothing is filed or routed. `lint-allow: <reason>` on a line clears it.

The refusal boundary: already covered, [[secret-governance]] § The capture boundary: refuse before anything is written.

Source: [[script-kb-capture]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: "$KB_ENGINE_ROOT/scripts/kb-capture" --bogus >/dev/null 2>&1; [ $? -eq 2 ] && grep -qF 'Usage: kb-capture [--title TITLE] [--into TREE] [--from FILE] [DIR]' "$KB_ENGINE_ROOT/scripts/kb-capture"

## kb-catalog

Purpose: "one compact catalog of the registered libraries, for triage (#130)." For each registered library it prints `<name>  [<scope>]  <path>`, then `covers:` (the root `INDEX.md` description), `not covered:` (its "Not covered here:" line, or "(none stated)") and `topics:` (the slugs its root router lists, `sources/` left out).

Usage: `kb-catalog`

- No arguments, no flags and no defaults to set.
- Exit codes: "Exit 0, or 2 on a usage error or an unreadable registry." Observed: any argument prints `usage: kb-catalog   (takes no arguments)` and exits 2; with no library registered it says so and exits 0.
- Never: "Read-only. It reads each served library's root INDEX.md and nothing else: the catalog is built when it is asked for, so it cannot go stale." A registered library that is not served is printed as `! <name>: <state> at <path> — not served`, "never dropped".

How triage uses it: already covered, [[retrieval-agent-behavior]] § A question goes to a triage desk, which sends it to one library's specialist.

Source: [[script-kb-catalog]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: t=$(mktemp -d); r=0; HOME="$t" "$KB_ENGINE_ROOT/scripts/kb-catalog" >/dev/null 2>&1 || r=1; HOME="$t" "$KB_ENGINE_ROOT/scripts/kb-catalog" x >/dev/null 2>&1; [ $? -eq 2 ] || r=1; rm -rf "$t"; exit $r

## kb-due

Purpose: "which knowledge trees are due for a hygiene sweep, and (with --nudge) everything else a librarian should act on."

Usage: `kb-due [tree ...] [--quiet|--nudge [--summary] [--summary-out FILE] [--session]]`

- It scans every library registered on this machine plus any further trees given (a project `knowledge/` tree).
- `--quiet` prints only overdue trees, "and nothing at all when none are (sweep only)". `--nudge` prints the full librarian nudge and "nothing at all when nothing in any category is due". `--summary`, with `--nudge`, also prints as the last line the nudge condensed to one line beginning "KB due — ". `--summary-out FILE` writes what the person is shown to FILE: the summary line, after the warning when the `kb.nudge` setting is one it does not understand, and empty when there is nothing to show; the nudge body still goes to stdout, "so the caller needs no text matching". `--session` is for the session-start hook: it decides from `kb.nudge` (the registry's, or `$KB_NUDGE` over it: `always`, `weekly:<day>` or `off`) and today whether to show the nudge, before computing anything, and its stdout is what the hook adds to the model's context; "It is the schedule's one home (#253)".
- Each library's line leads with its assertion coverage, and which items a line holds, and in what order, is decided by `Attention.items` in `kbengine.attention` (#253): already covered, [[agent-lifecycle-roles]] § Cadence is surfaced at session start. `kb-due` words the items, no longer loads the `kb-report` script, and reads where the reports live from `kbengine.reports`.
- Defaults: `sweep_interval:` absent means 30d. It reads `last_swept:` and `sweep_interval:` from each tree's `INDEX.md`.
- Exit codes: "Exit 0 always: being due is a nudge, never a failure."
- Never: no assertion is ever run or even read; only a count and the command (`kb-verify <path> --yes`) are printed. A failure computing one category for one tree is a one-line warning to stderr and that category is skipped for that tree only: "This hook must never crash a session."

What each item counts, the archived and read-only rules and the nudge setting: already covered, [[agent-lifecycle-roles]] § Cadence is surfaced at session start, and may be prepared on a schedule and the sections beneath it.

Source: [[script-kb-due]] · [[engine-changelog-v0.6.0]] (#239, #253)
Verified: 2026-11-24 · by: agent · method: doc-review
Recheck: t=$(mktemp -d); r=0; HOME="$t" "$KB_ENGINE_ROOT/scripts/kb-due" --nudge "$t/none" >/dev/null 2>&1 || r=1; grep -qF 'Exit 0 always: being due is a nudge, never a failure.' "$KB_ENGINE_ROOT/scripts/kb-due" || r=1; grep -qF 'Usage: kb-due [tree ...] [--quiet|--nudge [--summary] [--summary-out FILE] [--session]]' "$KB_ENGINE_ROOT/scripts/kb-due" || r=1; rm -rf "$t"; exit $r
Recheck: t=$(mktemp -d); r=0; d="$t/fxs"; mkdir -p "$d"; printf -- '---\nname: x-index\ndescription: d\nmemory_type: reference\ndomain: fxs\nscope: general\nfreshness_horizon: 90d\nverifier_budget: 5\nsweep_interval: 30d\nkb_contract: "2.7"\nlast_swept: never\n---\n# x\n\n- \133f](f.md) — f\n' > "$d/INDEX.md"; printf -- '---\nname: f\ndescription: d\nmemory_type: semantic\ndomain: fxs\nscope: general\nverified: 2026-11-20\nmetadata:\n  type: fact\n  node_type: memory\n  created: 2026-01-01\ntags: [x]\nkeywords: [x]\n---\n# f\n\n## A\n\nfact.\n\nSource: \133\133f]]\nVerified: 2026-11-20 · by: agent · method: manual\nRecheck: true\n\n## B\n\nfact.\n\nSource: \133\133f]]\nVerified: 2026-11-20 · by: agent · method: manual\n' > "$d/f.md"; k="$KB_ENGINE_ROOT/scripts/kb-due"; export HOME="$t" KB_TODAY=2026-11-24; o=$(KB_NUDGE=always "$k" --nudge --session --summary-out "$t/sum" "$d" 2>&1); printf '%s\n' "$o" | grep -qF 'fxs: assertion coverage 1 of 2 sourced sections; sweep due' || r=1; grep -qF 'KB due — ' "$t/sum" || r=1; o=$(KB_NUDGE=off "$k" --nudge --session --summary-out "$t/sum2" "$d" 2>&1); printf '%s\n' "$o" | grep -qF 'fxs:' && r=1; printf '%s\n' "$o" | grep -qF 'kb.nudge=off' || r=1; [ ! -s "$t/sum2" ] || r=1; rm -rf "$t"; exit $r

## kb-eval-sample

Purpose: "a graded retrieval sample, joined with what each case cost (#97)." It draws a seeded sample of a library's positive and cross-root golden cases, a person puts each to a fresh `kb-retrieve` agent, and the runs are graded with `grade-routing` and `grade-eval --transcripts`.

Usage, as written:

```
kb-eval-sample prepare <library> [--seed N] [--n K] [--cases Q<n>,...] --out DIR
kb-eval-sample grade DIR [--projects DIR]
```

- Defaults: "--seed today as YYYYMMDD, --n 5."
- `prepare` writes `<out>/sample.tsv` and `<out>/PROMPTS.md`. `grade` finds each case's transcript by its description (the newest, if several), writes `<out>/mapping`, runs both graders and prints a cost row per case. `--cases Q4,Q9` draws exactly those cases.
- Exit codes: "Exit for grade: 0 every graded case passed both graders, 1 any failed, 2 nothing to grade."

How the sample is used and what it found: already covered, [[retrieval-eval-integrity]] § Cost is measured from transcripts already on disk, and a sample is graded from the same transcripts.

Source: [[script-kb-eval-sample]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'Exit for grade: 0 every graded case passed both graders, 1 any failed, 2 nothing to grade.' "$KB_ENGINE_ROOT/scripts/kb-eval-sample" && grep -qF 'Defaults: --seed today as YYYYMMDD, --n 5.' "$KB_ENGINE_ROOT/scripts/kb-eval-sample"

## kb-init

Purpose: "scaffold a project knowledge tree. No model required." "GOVERNING RULE: create what is missing, touch nothing that exists." In a project that is already initialised, such as a fresh clone, it scaffolds nothing and only wires the hook, because `core.hooksPath` "is local config that git does not clone".

Usage: `kb-init [--name NAME] [DIR]`

- `DIR` is any directory inside the target project (default: current directory). "The project root is the enclosing git repository, not DIR itself."
- `--name` is the project name (default: the repository's directory name), kebab-cased.
- Exit codes: the header states none. Observed: 0 on a first run and on a re-run; 2 when `DIR` is not a directory or no name can be derived.
- What it creates, observed in a fixture run: `knowledge/INDEX.md` (scope `repo:<name>`, `freshness_horizon: 14d`, and since #236 `kb_contract: "2.7"`), `.claude/`, `CLAUDE.md` and `.githooks/pre-commit`. Each line of its output is `created`, `kept` or `note`. A second run reports `kept` for each.
- Never destructive: "a scaffolder that is destructive on its second run is one nobody runs twice."

The project-tree hook and why a clone needs `kb-init`: already covered in part, [[guardrail-verification]] § A guardrail can be skipped without anyone being told.

Source: [[script-kb-init]] · [[engine-changelog-v0.6.0]] (#236, the `kb-init` sentence)
Verified: 2026-11-24 · by: agent · method: doc-review
Recheck: t=$(mktemp -d); r=0; export GIT_CONFIG_GLOBAL=/dev/null GIT_CONFIG_NOSYSTEM=1; git init -q "$t/p"; "$KB_ENGINE_ROOT/scripts/kb-init" "$t/p" >/dev/null 2>&1 || r=1; grep -qx 'scope: repo:p' "$t/p/knowledge/INDEX.md" || r=1; grep -qx 'kb_contract: "2.7"' "$t/p/knowledge/INDEX.md" || r=1; "$KB_ENGINE_ROOT/scripts/kb-init" "$t/p" 2>&1 | grep -qF 'kept      knowledge/INDEX.md' || r=1; "$KB_ENGINE_ROOT/scripts/kb-init" "$t/none" >/dev/null 2>&1; [ $? -eq 2 ] || r=1; rm -rf "$t"; exit $r

## kb-libraries

Purpose: "the one reader of the per-machine library registry (ADR-0014)." The reader itself is `kbengine.registry`; this script is the CLI over it and owns "terms, origins, links and library_home".

Usage, as written:

```
kb-libraries          one line per registered library: <state>TAB<name>TAB<path>
kb-libraries --read-only   one line per read-only library: <name>TAB<path>TAB<origin>
kb-libraries --linked PATH...   one line per PATH reached through a link: <path>TAB<real path>
```

- Defaults: the registry is `~/.config/ai-kb/libraries`, in git-config format. The path "is FIXED and does not follow XDG_CONFIG_HOME".
- The states are `ok`, `missing`, `not-a-library`, `name-mismatch` and `unsupported-contract`; "only `ok` is served".
- Exit codes: "Exit 0, or 2 if the registry exists and git cannot read it." Observed: an unknown option prints `usage: kb-libraries [--read-only]` and exits 2.
- Never: every state other than `ok` is reported by whoever reads this, "never dropped". `--linked` names a file reached through a link and never refuses.

The states, the registry and why it is fixed: already covered, [[design-thesis]] § A library is installed on a machine by being registered there.

Source: [[script-kb-libraries]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: t=$(mktemp -d); r=0; HOME="$t" "$KB_ENGINE_ROOT/scripts/kb-libraries" >/dev/null 2>&1 || r=1; HOME="$t" "$KB_ENGINE_ROOT/scripts/kb-libraries" --bogus >/dev/null 2>&1; [ $? -eq 2 ] || r=1; grep -qF 'Exit 0, or 2 if the registry exists and git cannot read it.' "$KB_ENGINE_ROOT/scripts/kb-libraries" || r=1; rm -rf "$t"; exit $r

## kb-library

Purpose: "register, unregister and list the domain libraries on this machine (ADR-0014)." "Registering NEVER writes to the library."

Usage, as the script prints it:

```
kb-library add <path> | add <git-url> [--into DIR] [--read-only [--ref <tag>]] (<path> too) | remove <name> | list
                  | home | export | import <file> [--into DIR]
```

- Defaults: `home` is `$KB_LIBRARY_HOME`, else `kb.libraryHome` in the registry (`kb-bootstrap` records it), else `<engine>/kb`. `add <git-url>` clones into that home unless `--into DIR` is given, and never over an existing folder.
- Exit codes: "Exit 0 on success, 1 on a refusal, 2 on a usage error."
- Never: `remove` "Never deletes files". The overlap warning at `add` "is advice: it never refuses, never changes the exit status, and writes nothing." The registry "is read only through kb-libraries; this is the only thing that writes it."
- Overlap: two libraries are neighbours when they share at least `MIN_SHARED = 3` terms and those terms are at least `MIN_RATIO = 0.25` of the smaller library's terms.

Read-only registration, pinning with `--ref`, export and import: already covered, [[releases-and-contract-versioning]] § A consumer's copy is registered read-only: served, never curated here and § A read-only copy can be pinned to a release tag. The overlap warning: [[retrieval-agent-behavior]] § Neighboring libraries are separated by a scope line, and registration warns of an overlap.

Source: [[script-kb-library]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: t=$(mktemp -d); r=0; HOME="$t" "$KB_ENGINE_ROOT/scripts/kb-library" list >/dev/null 2>&1 || r=1; HOME="$t" "$KB_ENGINE_ROOT/scripts/kb-library" >/dev/null 2>&1; [ $? -eq 2 ] || r=1; grep -qF 'Exit 0 on success, 1 on a refusal, 2 on a usage error.' "$KB_ENGINE_ROOT/scripts/kb-library" || r=1; rm -rf "$t"; exit $r

## kb-owner

Purpose: "who owns a file, by CODEOWNERS, deterministically." It implements GitHub's rule: "the LAST matching pattern wins, not the most specific."

Usage, as written:

```
kb-owner <repo-root> <path> [<path>...]
kb-owner <repo-root> --list         every pattern, in file order
```

- Patterns supported: `*` (any), a leading `/` anchoring to the root, a trailing `/` matching a directory and everything under it, `*` and `?` within a segment, and `**`.
- Defaults: "An unowned file resolves to `unassigned` — never to a guess."
- Exit codes: "Exit 0 always; unowned paths print `unassigned`." Observed: with no path it prints a usage line and still exits 0. Output is one `path`, a tab, then the owner per line.
- Not enforcement: "DORMANT AS ENFORCEMENT: CODEOWNERS is advisory until branch protection makes owner review a required check (ADR-0002)."

Why owners are dormant: already covered, [[agent-lifecycle-roles]] § Mutation governance.

Source: [[script-kb-owner]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: t=$(mktemp -d); r=0; printf '* @team-a\n/scripts/ @team-b\n' > "$t/CODEOWNERS"; o=$("$KB_ENGINE_ROOT/scripts/kb-owner" "$t" scripts/x docs/y); [ $? -eq 0 ] || r=1; printf '%s\n' "$o" | grep -q 'scripts/x.@team-b' || r=1; printf '%s\n' "$o" | grep -q 'docs/y.@team-a' || r=1; printf '/scripts/ @team-b\n* @team-a\n' > "$t/CODEOWNERS"; "$KB_ENGINE_ROOT/scripts/kb-owner" "$t" scripts/x | grep -q 'scripts/x.@team-a' || r=1; printf '/scripts/ @team-b\n' > "$t/CODEOWNERS"; "$KB_ENGINE_ROOT/scripts/kb-owner" "$t" docs/y | grep -q 'unassigned' || r=1; rm -rf "$t"; exit $r

## kb-prepare

Purpose: "draft the legwork of kb-distill or kb-audit unattended, as a review packet (ADR-0017, #99)." "The decision stays attended."

Usage: `kb-prepare distill|audit <library> [--out DIR] [--budget-usd N] [--claude PATH] [--force]`

- Defaults: `--out DIR` is kb-apply's packets directory, and is refused inside the library. `--budget-usd N` is "the model run's dollar cap (default 1.00)". `--claude PATH` is "the claude binary (default: $KB_CLAUDE, then PATH)".
- It runs a model only when the work is due; `--force` drafts anyway.
- Output: it prints the packet's path, never its text.
- Exit codes: "Exit 0 nothing to decide (not due, or the run proposed nothing), 1 a packet is waiting for a decision, 2 could not run."
- Never: the model is `claude -p` given Read, Grep and Glob and no other tool, "so it cannot change a library or send what it reads anywhere".

Why a headless run cannot write: already covered, [[unattended-preparation]] § A headless run cannot write, and that is enforcement rather than instruction.

Source: [[script-kb-prepare]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: "$KB_ENGINE_ROOT/scripts/kb-prepare" >/dev/null 2>&1; [ $? -eq 2 ] && grep -qF '1 a packet is waiting for a decision, 2 could not run.' "$KB_ENGINE_ROOT/scripts/kb-prepare" && grep -qF -- '--budget-usd N  the model run' "$KB_ENGINE_ROOT/scripts/kb-prepare"

## kb-report

Purpose: "the weekly report of what needs a person's decision, and nothing else (ADR-0017, #102)."

Usage: `kb-report [tree ...] [--out DIR]`

- Defaults: with no tree, every registered library. The report is `<out>/<ISO year>-W<ISO week>.md` and a rerun in the same week replaces it. The default `<out>` is `$KB_REPORTS`, or `~/.local/state/ai-kb/reports` (ADR-0021).
- Exit codes: "Schedulable (#95): exit 0 nothing to decide, 1 decisions written, 2 could not run (the engine's readers would not load, or the report's own folder is inside a library). A library that cannot be read is an item under its name, not a failure."
- Never: "It never holds a fact's, a source's or a packet's text: paths, counts, headings and ids only." The file is written even when there is nothing to decide, "so a quiet week can be told from a week the job did not run".

The report opens with assertion coverage per library (#239), covered in [[unattended-preparation]] § The weekly report lists decisions and holds no knowledge. `kb-report` filters on `shelved` as `kb-due` does, before anything else about the library is read, and takes where the reports live from `kbengine.reports` (#253).

What the report lists: already covered, [[unattended-preparation]] § The weekly report lists decisions and holds no knowledge and the sections beneath it.

Source: [[script-kb-report]] · [[engine-changelog-v0.6.0]] (#239, #253)
Verified: 2026-11-24 · by: agent · method: doc-review
Recheck: t=$(mktemp -d); r=0; HOME="$t" KB_REPORTS="$t/rep" "$KB_ENGINE_ROOT/scripts/kb-report" >/dev/null 2>&1 || r=1; ls "$t/rep" | grep -qE '^[0-9]{4}-W[0-9]{2}\.md$' || r=1; grep -qF 'exit 0 nothing to decide, 1 decisions' "$KB_ENGINE_ROOT/scripts/kb-report" || r=1; rm -rf "$t"; exit $r
Recheck: t=$(mktemp -d); r=0; d="$t/fxr"; mkdir -p "$d"; printf -- '---\nname: x-index\ndescription: d\nmemory_type: reference\ndomain: fxr\nscope: general\nfreshness_horizon: 90d\nverifier_budget: 5\nsweep_interval: 30d\nkb_contract: "2.7"\nlast_swept: 2026-11-20\n---\n# x\n\n- \133f](f.md) — f\n' > "$d/INDEX.md"; printf -- '---\nname: f\ndescription: d\nmemory_type: semantic\ndomain: fxr\nscope: general\nverified: 2026-11-20\nmetadata:\n  type: fact\n  node_type: memory\n  created: 2026-01-01\ntags: [x]\nkeywords: [x]\n---\n# f\n\n## A\n\nfact.\n\nSource: \133\133f]]\nVerified: 2026-11-20 · by: agent · method: manual\nRecheck: true\n\n## B\n\nfact.\n\nSource: \133\133f]]\nVerified: 2026-11-20 · by: agent · method: manual\n' > "$d/f.md"; HOME="$t" KB_TODAY=2026-11-24 "$KB_ENGINE_ROOT/scripts/kb-report" "$d" --out "$t/rep" >/dev/null 2>&1; [ $? -eq 0 ] || r=1; grep -qF 'assertion coverage 1 of 2 sourced sections' "$t/rep/"*.md || r=1; rm -rf "$t"; exit $r

## kb-schedule

Purpose: "the opt-in weekly job that keeps libraries current (ADR-0017, #103)." "Nothing runs unless the user installs it."

Usage, as written:

```
kb-schedule install [--backend systemd|cron] [--prepare] [--budget-usd N] [--claude PATH]
                           [--watch ALLOWLIST|none]
kb-schedule status | remove | run
```

- Defaults: `--prepare` is off ("also draft review packets with a headless, read-only model run (default: off)"). `--budget-usd N` is "the dollar cap per model run (default 1.00)". `--claude PATH` is "the one on PATH now". `--watch FILE` is "the engine's watcher/allowlist.json, if any".
- The job is a systemd user timer on Monday at 07:00, or a crontab line where there is no systemd.
- Exit codes: "status exits 0 installed, 1 not installed. run exits 0 when every step ran (findings included), 2 when a step could not run. install and remove exit 0, or 2 on a usage or system error."
- Never: "No fact is edited: every step queues, drafts or reports." The log "records each step's exit code and nothing a step printed". `install` records the PATH it was run with in `schedule.conf`; reinstall to change it.

The steps and their order, and what the schedule costs: already covered, [[unattended-preparation]] § The schedule is opt-in and edits no fact.

Source: [[script-kb-schedule]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: t=$(mktemp -d); HOME="$t" KB_SYSTEMCTL=/bin/false KB_CRONTAB=/bin/false "$KB_ENGINE_ROOT/scripts/kb-schedule" status >/dev/null 2>&1; r=$?; rm -rf "$t"; [ $r -eq 1 ] && grep -qF 'status exits 0 installed, 1 not installed.' "$KB_ENGINE_ROOT/scripts/kb-schedule"

## kb-session-start

Purpose: "SessionStart hook: resolve the knowledge engine and its libraries." It is registered by `kb-bootstrap` and lives in the engine, so it is versioned with it.

Usage: the header gives no usage line. Its one option is `--subagent`: "the same context as a SubagentStart event, for kb-retrieve", with the librarian's nudge left out.

- Output: a JSON object with `additionalContext` and, when there is something for the person, a `systemMessage`. Agents receive resolved absolute paths.
- Defaults: the nudge is shown `always` unless `kb.nudge` in the registry, or `$KB_NUDGE` over it, says `weekly:<day>` or `off`. The hook no longer decides that itself (#253): it calls `kb-due <project tree> --nudge --session --summary-out FILE` and prints what comes back, so it matches no `KB due — ` text and works out no reports folder. If `kb-due` fails it says so in one line, with its exit code: "kb-due failed (exit N), so the knowledge-base nudge is unavailable this session." That line goes to the model and to the person, where a failed `kb-due` showed no nudge before.
- Exit codes: the header states none. Observed: 0 on a normal start. The header says: "Absence is graceful at every level."
- Never: "No engine, no libraries, or neither — the session still starts, and says so."

How the nudge is gated and what a subagent is given: already covered, [[agent-lifecycle-roles]] § When the nudge is shown, and a contract it cannot read and [[retrieval-agent-behavior]] § A retrieval subagent is given the engine and the library list by a hook.

Source: [[script-kb-session-start]] · [[engine-changelog-v0.6.0]] (#253)
Verified: 2026-11-24 · by: agent · method: doc-review
Recheck: t=$(mktemp -d); r=0; o=$(HOME="$t" "$KB_ENGINE_ROOT/scripts/kb-session-start" </dev/null); [ $? -eq 0 ] || r=1; printf '%s' "$o" | grep -qF '"hookEventName":"SessionStart"' || r=1; HOME="$t" "$KB_ENGINE_ROOT/scripts/kb-session-start" --subagent </dev/null | grep -qF '"hookEventName":"SubagentStart"' || r=1; grep -qF -- '--nudge --session --summary-out' "$KB_ENGINE_ROOT/scripts/kb-session-start" || r=1; rm -rf "$t"; exit $r

## kb-sources

Purpose: "do the external URLs the facts cite still resolve? Reports and queues; never edits."

Usage: `kb-sources <library-or-tree> [--yes]`

- `<library-or-tree>` is a library root (has `INDEX.md`), or a directory of libraries (`kb/`). `--yes` approves this run's network access: fetch each unique URL once.
- Defaults: without `--yes` it "lists every URL, its stub and its dependants, says nothing was fetched, and exits 0". Timeout 10s.
- Exit codes: "Exit 0 when every URL is alive (or nothing was fetched), 1 when any was dead or unreachable, 2 on usage error."
- Never: "NOTHING IS FETCHED WITHOUT --yes." "DETECTION ONLY. A dead URL never edits a fact, never edits a stub, and never marks a section `provenance: BROKEN`." A stub marked `- **Liveness:** manual` is never fetched.

The request sequence, classification and sign-in rule: already covered, [[agent-lifecycle-roles]] § The source-URL liveness check as built and its sub-sections.

Source: [[script-kb-sources]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: "$KB_ENGINE_ROOT/scripts/kb-sources" >/dev/null 2>&1; [ $? -eq 2 ] && grep -qF 'Exit 0 when every URL is alive (or nothing was fetched), 1 when any was dead or unreachable,' "$KB_ENGINE_ROOT/scripts/kb-sources"

## kb-telemetry

Purpose: "what kb-retrieve costs in normal use, read from transcripts already on disk (#97)."

Usage, as written:

```
kb-telemetry [--since YYYY-MM-DD] [--until YYYY-MM-DD] [--window Nd] [--library NAME]
                    [--projects DIR] [--include-evals] [--not-found]
```

- Defaults: "the last 28 days, in 7-day windows; every library; $CLAUDE_CONFIG_DIR/projects, else ~/.claude/projects." Eval runs are left out unless `--include-evals`.
- Exit codes: "Schedulable (#95): exit 0, or 2 on a usage error."
- Never: "Read-only. By default it prints paths, counts and medians, never a question, an answer or a file's text." `--not-found` prints the question of each such run, never an answer or a file's text.

The five measures and the not-found runs: already covered, [[retrieval-eval-integrity]] § Cost is measured from transcripts already on disk, and a sample is graded from the same transcripts and § The runs that found nothing, and what it prints.

Source: [[script-kb-telemetry]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: t=$(mktemp -d); r=0; HOME="$t" CLAUDE_CONFIG_DIR="$t/cc" "$KB_ENGINE_ROOT/scripts/kb-telemetry" >/dev/null 2>&1 || r=1; HOME="$t" "$KB_ENGINE_ROOT/scripts/kb-telemetry" --bogus >/dev/null 2>&1; [ $? -eq 2 ] || r=1; rm -rf "$t"; exit $r

## kb-verify

Purpose: "re-run the assertions that defend stale facts. Reports; applies only when told."

Usage: `kb-verify <library-or-tree> [--yes] [--apply] [--all] [--budget N] [--unattended]`

- `--yes` runs the commands; without it they are printed and nothing runs, "and nothing is queued either". `--apply` restamps sections whose assertion passed (today, `by: agent`, `method: query`). `--all` is every section with an assertion, not only the stale ones. `--budget N` overrides the domain's `verifier_budget`. `--unattended` skips `Recheck (attended):` assertions and runs the rest with an empty HOME, and "Refuses --apply".
- Defaults: report only. The default budget when none is declared is in [[agent-lifecycle-roles]] § The default budget.
- Selection is by the lease the evidence sets, not by `by:` (ADR-0022): already covered, [[agent-lifecycle-roles]] § A stamp's lease follows its evidence, not its author. The header reads `short lease Nd` where it read `agent TTL Nd`; `--apply` still restamps "today, `by: agent`, `method: query`".
- Exit codes: "Exit 0 when nothing failed, 1 when an assertion failed, 2 on usage error."
- Never: "A FAILURE is never applied, with or without --apply." A pass refreshes nothing unless `--apply` is passed.

The three rules, selection by staleness and what is read: already covered, [[agent-lifecycle-roles]] § The Verifier as built and the sub-sections beneath it.

Source: [[script-kb-verify]] · [[engine-changelog-v0.6.0]] (#236, #238, #249, #250)
Verified: 2026-11-24 · by: agent · method: doc-review
Recheck: "$KB_ENGINE_ROOT/scripts/kb-verify" >/dev/null 2>&1; [ $? -eq 2 ] && grep -qF 'Exit 0 when nothing failed, 1 when an assertion failed, 2 on usage error.' "$KB_ENGINE_ROOT/scripts/kb-verify"
Recheck: t=$(mktemp -d); r=0; mkdir "$t/l"; printf -- '---\nname: x-index\ndescription: d\nmemory_type: reference\ndomain: x\nscope: general\nfreshness_horizon: 90d\nsweep_interval: 30d\nlast_swept: never\n---\n# x\n' > "$t/l/INDEX.md"; o=$("$KB_ENGINE_ROOT/scripts/kb-verify" "$t/l" 2>&1); printf '%s\n' "$o" | grep -qF 'short lease 9d' || r=1; printf '%s\n' "$o" | grep -qF 'agent TTL' && r=1; rm -rf "$t"; exit $r

## kb-watch

Purpose: "the Watcher: currency checker for external/general facts. Detects; never edits a fact."

Usage, as written:

```
kb-watch <allowlist.json> [--dry-run]
kb-watch --dump <needs-attention.md>
```

- `--dump` prints every record the script can parse out of a queue file, one per line, tab-separated. `--dry-run` is "the explicit opt-out for a human who wants to see what would be queued first".
- Defaults: "DEFAULT IS WRITE, NOT --dry-run." Relative paths in the allowlist resolve against the allowlist file's own directory.
- Exit codes: "Exit 0 when no source drifted or was unreachable, 1 when any did (whether newly queued or already in the queue), 2 on a usage or config error (bad JSON, a malformed entry, a library with no INDEX.md) — the run still processes every well-formed entry first."
- Never: it "imports no network module" and fetches nothing; no model reads the page. Old and new values are printed only at a terminal or with `--dry-run`.

The design and safety reasoning: already covered, [[agent-lifecycle-roles]] § The Watcher as built and § Watcher safety design.

Source: [[script-kb-watch]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: "$KB_ENGINE_ROOT/scripts/kb-watch" >/dev/null 2>&1; [ $? -eq 2 ] && grep -qF 'Exit 0 when no source drifted or was unreachable, 1 when any did (whether newly queued or already' "$KB_ENGINE_ROOT/scripts/kb-watch"

## The kbengine package

`scripts/kbengine/` is a Python package of readers the scripts share, so that "a version means one thing in all of them". The scripts import it as `from kbengine import …`, and no script edits `sys.path` but one: `check-golden` inserts its own folder, "found beside it", because a regression case loads that script bare. The package sits in the folder beside them. Every module is "Stdlib only". It is not a command. The one command-line use is `python3 -m kbengine.env`, which prints the variable table, `--names` one name per line, and `--path NAME` one resolved location.

Each module's purpose, from its docstring:

- `attention`: "What needs a person's attention in one library: the one reading of every category kb-due, kb-report and kb-prepare each used to compute for themselves (#204)." Since #253 it also holds the policy: `Attention.items` is a frozen, ordered list per library (coverage first), each item saying whether it is a decision, a field that failed being an item; `kb-due` and `kb-report` word the items and test no condition to find them. `manual_stubs` is a kind only the weekly report says.
- `citations`: "Citation resolution: which file a cited path names, across the libraries registered on this machine (#252)."
- `clock`: "What day it is, for the engine's staleness and sweep logic (#205)." `today()` is `$KB_TODAY` when set, else the real date; a malformed `$KB_TODAY` raises rather than falling back.
- `contract`: "The contract versions (CONVENTIONS §14, ADR-0016): which this engine knows, what a library's declared `kb_contract:` means against them, and how a version is written."
- `env`: "What the engine reads from the environment, declared once, and where its files are (ADR-0021, #205)."
- `factfiles`: "The fact-file reader: which files of a library are knowledge, what kind each is, and what the sections of one say." Since #251 `tree(root)` returns the library's files, each read and parsed the first time it is asked for and never again, and `tree_from(blobs)` builds the same value from git blobs; `check-kb`'s `kb_files`, `unsourced`, `unstamped_keys`, `marked_sections` and `reachable` live here, and so does the "not CONFLICTED" rule, named `served`, beside `in_scope`. `factfiles.FENCE` is the one fence definition (#257) and `factfiles.EXCLUDED_DIRS` the one rule naming the folders that are not the knowledge base, used by `check-xlinks`, `check-scope`, `check-blast` and `kb-library`.
- `golden`: "The golden set: a library's own answer key, its record format, its rules and its grading (#252, CONVENTIONS §10)."
- `lease`: "The lease of every stamp in a library (#250, ADR-0022): the one place that says whether a stamp is asserted, what lease it is on, whether it is due or stale, and how much of the library is asserted."
- `library`: "A library's root INDEX.md, read once and the same way by every script that needs it." Frontmatter only. It exposes one short-lease value, `unasserted_ttl`, whichever field supplied it (`unasserted_ttl:` from contract 2.7, `agent_ttl:` below it).
- `lint`: "The assertion lint (#249, from kb-verify; ADR-0022): which Recheck: assertions cannot fail, and which reach outside the library, as structured findings and as the warnings kb-verify prints."
- `queue`: "The needs-attention queue: how a finding is written to a library's needs-attention.md, and how the queue is read back."
- `registry`: "The per-machine library registry, read once and the same way by every script that needs it (ADR-0014, ADR-0020, #210)."
- `reports`: "Where the weekly reports live, which is the latest, and whether it is current (ADR-0017, ADR-0021, #253)."
- `transcripts`: "What an agent answered, read from its subagent transcript (#252)."

`__init__.py` is empty. What each module replaced and why: already covered in part, [[design-thesis]] § One module declares every variable the engine reads and § One reader serves a library's root INDEX.md, and [[guardrail-verification]] § The engine's date can be injected, so a test does not wait on the calendar.

Source: [[scripts-kbengine-package]] · [[engine-changelog-v0.6.0]] (#249 to #253, #257)
Verified: 2026-11-24 · by: agent · method: doc-review
Recheck: t=$(mktemp -d); r=0; [ "$(cd "$KB_ENGINE_ROOT/scripts" && grep -l 'sys\.path' * 2>/dev/null | tr '\n' ' ')" = 'check-golden ' ] || r=1; grep -qF 'What needs a person'"'"'s attention in one library: the one reading of every category kb-due, kb-report' "$KB_ENGINE_ROOT/scripts/kbengine/attention.py" || r=1; grep -qF 'Citation resolution: which file a cited path names, across the libraries registered on this' "$KB_ENGINE_ROOT/scripts/kbengine/citations.py" || r=1; grep -qF 'What day it is, for the engine'"'"'s staleness and sweep logic (#205).' "$KB_ENGINE_ROOT/scripts/kbengine/clock.py" || r=1; grep -qF 'The contract versions (CONVENTIONS §14, ADR-0016): which this engine knows, what a library'"'"'s' "$KB_ENGINE_ROOT/scripts/kbengine/contract.py" || r=1; grep -qF 'What the engine reads from the environment, declared once, and where its files are (ADR-0021, #205).' "$KB_ENGINE_ROOT/scripts/kbengine/env.py" || r=1; grep -qF 'The fact-file reader: which files of a library are knowledge, what kind each is, and what the' "$KB_ENGINE_ROOT/scripts/kbengine/factfiles.py" || r=1; grep -qF 'The golden set: a library'"'"'s own answer key, its record format, its rules and its grading (#252,' "$KB_ENGINE_ROOT/scripts/kbengine/golden.py" || r=1; grep -qF 'The lease of every stamp in a library (#250, ADR-0022): the one place that says whether a stamp is' "$KB_ENGINE_ROOT/scripts/kbengine/lease.py" || r=1; grep -qF 'A library'"'"'s root INDEX.md, read once and the same way by every script that needs' "$KB_ENGINE_ROOT/scripts/kbengine/library.py" || r=1; grep -qF 'The assertion lint (#249, from kb-verify; ADR-0022): which Recheck: assertions cannot fail,' "$KB_ENGINE_ROOT/scripts/kbengine/lint.py" || r=1; grep -qF 'The needs-attention queue: how a finding is written to a library'"'"'s needs-attention.md, and how' "$KB_ENGINE_ROOT/scripts/kbengine/queue.py" || r=1; grep -qF 'The per-machine library registry, read once and the same way by every script that needs it' "$KB_ENGINE_ROOT/scripts/kbengine/registry.py" || r=1; grep -qF 'Where the weekly reports live, which is the latest, and whether it is current (ADR-0017, ADR-0021,' "$KB_ENGINE_ROOT/scripts/kbengine/reports.py" || r=1; grep -qF 'What an agent answered, read from its subagent transcript (#252).' "$KB_ENGINE_ROOT/scripts/kbengine/transcripts.py" || r=1; [ ! -s "$KB_ENGINE_ROOT/scripts/kbengine/__init__.py" ] || r=1; PYTHONPATH="$KB_ENGINE_ROOT/scripts" python3 -I -c 'import sys; sys.path.insert(0, sys.argv[1]); from kbengine import attention, citations, clock, contract, env, factfiles, golden, lease, library, lint, queue, registry, reports, transcripts' "$KB_ENGINE_ROOT/scripts" || r=1; rm -rf "$t"; exit $r
