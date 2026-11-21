---
name: using-ai-kb
description: How to install and use ai-kb, the governed knowledge-base engine — setting up a machine (kb-bootstrap with no flag, --check and --apply), why every clone needs kb-init, registering a library (add, list, export and import, read-only, --ref), the session-start context and the nudge setting, the daily capture, distill and retrieve loop, adding a library or domain, what to do when something looks wrong, the engine's own hooks and test suite, contributing a change, and how the engine resolves its root.
memory_type: semantic
domain: knowledge-architecture
scope: general
verified: 2026-11-21
metadata:
  type: fact
  node_type: memory
  created: 2026-11-20
tags: [knowledge-architecture, how-to, install, contributing]
keywords: [ai-kb, install, set up, new machine, kb-bootstrap, --check, --apply, --library-home, KB_LIBRARY_HOME, kb-init, kb-library, add, list, remove, home, export, import, read-only, --ref, core.hooksPath, kb.engineRoot, KB_ENGINE_ROOT, session start, kb.nudge, KB_NUDGE, weekly, restart, fresh clone, engine not found, tests/run-checks, .githooks, owner-approved, CHANGELOG, Unreleased, contributing, CONTRIBUTING, root resolution, ADR-0001, ADR-0008, README, status, orphan]
---
# Using ai-kb: setting up, working day to day, and changing the engine

Each section answers one "how do I" question. The contract is `CONVENTIONS.md` in the engine, and "settles anything this guide leaves open" (CONTRIBUTING). What each script, skill and decision record does is held elsewhere in this library; this file gives the steps a person takes.

## What is in the engine repository, and where do the facts live?

The engine repository holds "the contract, the tooling and the behavior, and **no facts**" (README). Its paths:

| Path | What it is |
|---|---|
| `CONVENTIONS.md` | The contract: scope tiers, provenance, currency stamps, slugs and cross-links, contradictions, the retrieval rules. It settles any question this README leaves open. |
| `CONTRIBUTING.md` | The working guide: setting up a machine, the daily loop, curating, the weekly job, and working on the engine itself. |
| `plugins/knowledge-tools/` | The Claude Code plugin: five curation skills, the `kb-retrieve` agent, the `kb-ask` triage skill, and the `/kb-capture` and `/kb-init` commands. |
| `plugins/analysis-tools/` | A second plugin: a skill that reviews a work product for analytic rigor and coaches intelligence-analysis tradecraft. It reads its tradecraft from a library and holds none. |
| `scripts/` | Setup commands (`kb-bootstrap`, `kb-library`, `kb-init`, `kb-capture`), the upkeep commands (`kb-verify`, `kb-sources`, `kb-watch`, `kb-due`), the opt-in weekly job (`kb-schedule`, with `kb-prepare`, `kb-apply` and `kb-report`), `kb-backup`, the retrieval-cost tools (`kb-telemetry`, `kb-eval-sample`), and the checks that guard the contract. |
| `documents/decisions/` | Architecture decision records (ADR-0001 to ADR-0021). Each one explains why a rule exists. |
| `tests/run-checks` | A regression suite that breaks each check on purpose and requires it to notice. |

The facts live in two kinds of home. **Domain libraries**: each is its own git repository, one per subject domain, "cloned anywhere and registered on the machine with `kb-library add` (the engine's `kb/` is the default place)"; a library holds facts true wherever its subject appears, plus their sources and a golden set of test questions, and "Libraries may be private or public: the engine is public, and the knowledge doesn't have to be." **Project trees**: a `knowledge/` directory inside an ordinary project repository holds facts that are true only of that deployment. The separation is in [[design-thesis]].

Source: [[engine-readme]] · [[engine-contributing-schedulable]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'Each library is its own git repository, one per subject domain, cloned' "$KB_ENGINE_ROOT/README.md"

## How do I set up a new machine?

Requirements: git, bash, Python 3 (standard library only) and Claude Code. Then:

```bash
git clone https://github.com/dhtaylor/ai-kb.git ~/kb-engine      # the engine, anywhere you like
~/kb-engine/scripts/kb-bootstrap --apply                         # register the engine on this machine
~/kb-engine/scripts/kb-library add <library-git-url>             # each library you need
```

"The engine is wherever the script lives; nothing else is typed." Then restart your terminal and any open Claude Code session: "A shell profile is read at shell start, and Claude reads its settings and plugin content once per session." In each project where knowledge is to be captured, run `kb-init` in every clone (see below).

`kb-library add <git-url>` clones into the **library home** and registers the clone. The home is `<engine>/kb/` unless you choose another: `kb-bootstrap --apply --library-home <dir>` records it, and `KB_LIBRARY_HOME` overrides it for one shell. `kb-library home` prints where new clones go. If you have no library yet, the `kb-create-domain` skill stands one up.

Setting up a second machine with the same libraries is `kb-library export > libraries.txt` on the first and `kb-library import libraries.txt` on the second: "the export carries each library's name and origin, never a path." What export and import do for a read-only or pinned library is in [[releases-and-contract-versioning]]; what export does for a library with no remote is in [[design-thesis]].

Source: [[engine-readme]] · [[engine-contributing-schedulable]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'the export carries each library'"'"'s name and origin, never a path.' "$KB_ENGINE_ROOT/CONTRIBUTING.md"

## What do kb-bootstrap with no flag, --check and --apply each do?

```bash
kb-bootstrap               # reports drift, changes nothing, exits 1 if anything is out of step
kb-bootstrap --check       # the same, spelled out
kb-bootstrap --apply       # makes the changes above
```

With no flag, and with `--check`, it reports and changes nothing; the README says "Run `kb-bootstrap` (no flag, same as `--check`) at any time to see what it would change without changing anything." Only `--apply` makes the changes.

What `--apply` writes: `~/.claude/settings.json` (the engine path, the plugin, the session hook), `git config --global kb.engineRoot`, and a `PATH` block in your shell rc; it also points each registered library's local `core.hooksPath` at the engine's library hook. It "merges with what is already there, backs a file up before changing it, and is safe to run again — run it again whenever the engine moves."

Why it is one command, from the decision that made it (ADR-0008): it "writes every location from one source — its own path". Git hooks "resolve the engine through `git config --global kb.engineRoot`, after the environment", because git reads its global config "in every context that can produce a commit", which no shell startup file does. The shell block survives for `PATH`, "so a human can type `kb-init` — a convenience, not a dependency." The tests recorded in ADR-0008, on a throwaway `HOME` on 2026-09-22: `--check` before applying reported every item as would-set, exit 1, and wrote nothing; `--check` after applying reported all ok, exit 0; a second apply changed no file and wrote no backup. Why the default was changed to report-only is in [[guardrail-verification]].

Source: [[engine-readme]] · [[engine-contributing-schedulable]] · [[adr-0008-machine-bootstrap]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'kb-bootstrap               # reports drift, changes nothing, exits 1 if anything is out of step' "$KB_ENGINE_ROOT/CONTRIBUTING.md"

## Why does every clone need kb-init, and why might a fresh clone's commit checks not fire?

```bash
cd <project>
kb-init                   # new project: scaffolds knowledge/, .claude/ and a CLAUDE.md stanza
                          # already set up (you cloned it): wires the commit hook, nothing else
```

"Run `kb-init` in every clone, including one you cloned yesterday. `core.hooksPath` is local config that git does not clone, so a fresh clone runs *no* hook at all — observed: a file with a mismatched name, the wrong scope and no router line committed with exit 0 and no output whatsoever." ADR-0008 dates the observation 2026-09-21 and states the team instruction as "two steps: `kb-bootstrap` once per machine, `kb-init` once per clone." A machine-level command cannot wire a repository that has not been cloned yet, so `kb-init` in an already-initialised project "now wires the hook instead of refusing."

A library clone is wired differently: `kb-library add` "registers it under the name its `INDEX.md` declares and wires its clone to the engine's hook. It writes nothing into the library" ([[design-thesis]] holds why a library carries no hook).

A commit can also go through unchecked when the machine has no engine registered. The hook "deliberately skips rather than failing, so an unconfigured machine is not blocked from working, which does mean an unbootstrapped clone is unguarded." The fix for both is in "When something looks wrong" below. The decision that gave hooks a channel that does not depend on a shell is in [[guardrail-verification]].

Source: [[engine-readme]] · [[engine-contributing-schedulable]] · [[adr-0008-machine-bootstrap]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'so a fresh clone runs *no* hook at all' "$KB_ENGINE_ROOT/CONTRIBUTING.md"

## How do I register a library?

- **Add.** `kb-library add <git-url>` clones into the library home and registers the clone. `kb-library add <path>` registers a library you have already cloned anywhere. It registers the library in `~/.config/ai-kb/libraries` and wires its hook; "it writes nothing into the library itself."
- **List, remove, home.** `kb-library list` shows what is registered and whether each is servable. `kb-library remove <name>` unregisters one without deleting any file. `kb-library home` prints where new clones go.
- **Overlap warning.** If a new library's root `keywords:` and `tags:` overlap a registered library's enough to blur routing, `kb-library add` says so and names the neighbour. "It never refuses." Add a `Not covered here:` line to each library's root `INDEX.md` (CONVENTIONS §3) and the warning has done its job.
- **Export and import.** To another machine: see the setup section above.
- **A library someone else publishes**, which you read but do not curate, is added with `--read-only` (ADR-0020). It is served like any other, and session start marks it `(read-only)`. The library hook refuses commits in it; the commands and skills that refuse or skip it are listed in [[releases-and-contract-versioning]]. What you find in it is filed with its publisher; `kb-libraries --read-only` names its origin. "Re-add the same path without the flag to curate it here."
- **Pinned to a release tag**: `--ref` goes with `--read-only` — [[releases-and-contract-versioning]].
- **Missing libraries.** A machine that has not cloned a library "gets a warning, never a broken link: libraries are portable, and absence is not breakage." The session hook lists the registered libraries and "names any that went missing"; the states are in [[design-thesis]].

Source: [[engine-contributing-schedulable]] · [[engine-readme]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF '`kb-library remove <name>` unregisters one without deleting any file' "$KB_ENGINE_ROOT/CONTRIBUTING.md"

## How do I get the session-start context, and how do I configure the nudge?

Session start is the SessionStart hook, `scripts/kb-session-start` in the engine (ADR-0008 moved it there from `~/.claude/hooks/`). It injects the resolved engine path into the session context and, per ADR-0001, an unset or wrong root produces "a session-start warning and an instruction to report the root as unconfigured rather than guess." `kb-bootstrap --apply` installs the hook; restart a Claude Code session to get it.

Session start "tells you when something is due, and says nothing when nothing is." Ask for it yourself: `kb-due knowledge` ("every registered library, plus this project's tree"). What the nudge counts, when it is shown and what it hides is in [[agent-lifecycle-roles]].

To keep the nudge out of the other days when you work through the weekly report in one sitting, set `kb.nudge` in the machine registry (`$KB_NUDGE` overrides it):

```bash
git config --file ~/.config/ai-kb/libraries kb.nudge weekly:mon   # or: always (default), off
```

For `weekly:<day>` the day is `mon`..`sun`, "or the full name, any case". The values, and what is reported whatever the setting, are in [[agent-lifecycle-roles]]. A project's old `knowledge/` tree kept read-only can stop the nudge by declaring `archived: <date or reason>` in its root `INDEX.md` frontmatter; "This is a setting for `kb-due`, not part of the contract, and it does not retire a registered library: `kb-library remove` does."

The opt-in weekly job (`kb-schedule install`, `kb-schedule status`, `kb-schedule remove`) and its report are in [[unattended-preparation]].

Source: [[engine-contributing-schedulable]] · [[adr-0001-kb-root-resolution]] · [[adr-0008-machine-bootstrap]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'git config --file ~/.config/ai-kb/libraries kb.nudge weekly:mon   # or: always (default), off' "$KB_ENGINE_ROOT/CONTRIBUTING.md"

## How is ai-kb used day to day?

1. **Capture.** `/kb-capture` in a session, or `kb-capture` in a terminal, writes a dated note of what was done and seen. It writes it under `knowledge/episodic/` in the nearest tree and routes it, "unless the body holds a credential or personal identifier, in which case it is refused." "A capture records; it does not decide." Keep what you saw separate from what you concluded, and keep the hedges ("probably", "we didn't test that", "the docs say"). The title becomes the filename and the line a retrieval agent reads first: say what was done and seen, never what you concluded. Right: `Checkout 504s in staging; 47 idle-in-transaction connections; terminating them restored service`. Wrong: `Checkout 504s traced to idle connections exhausting the pool`.
2. **Distill.** `kb-distill` turns notes into durable facts. Observations become facts with a source and a verification stamp; speculation stays in the note. Facts land in the tier they belong to — "this deployment's quirk in the project tree, the product's behaviour in a library".
3. **Curate.** `kb-update-domain` folds in documents and specs. `kb-create-domain` and `kb-organize-domain` build and reshape domains. `kb-audit` sweeps for decay and reports without fixing anything.
4. **Retrieve.** The `kb-ask` skill picks the library a question belongs to from one compact catalog and hands it to a `kb-retrieve` specialist for that library. The specialist answers with citations and refuses to guess or to serve a disputed fact. Retrieval answers only from the knowledge base, "not from the code or config beside it"; "Reading the source is the caller's job; that is how a gap stays visible". How the agent decides where to look is in [[retrieval-agent-behavior]].

| You want to | Skill |
|---|---|
| Fold a document, spec or vendor page into a domain that exists | `kb-update-domain` |
| Stand up a domain that does not exist yet | `kb-create-domain` |
| Turn session notes into facts | `kb-distill` |
| Split, merge or rename files in a lumpy domain | `kb-organize-domain` |
| Sweep for decay and build a needs-attention queue | `kb-audit` |

Source: [[engine-readme]] · [[engine-contributing-schedulable]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'A capture **records; it does not decide**.' "$KB_ENGINE_ROOT/CONTRIBUTING.md"

## How do I add a new library or domain?

A library is its own repository, one domain, self-contained: "its facts, its sources, its archived evidence and its golden set travel together, so it stays useful wherever it is cloned." To add one:

1. Use the `kb-create-domain` skill to scaffold it.
2. Give it a remote.
3. On each machine that needs it, clone it anywhere and run `kb-library add <path>`. That registers it under the name its `INDEX.md` declares and wires its clone to the engine's hook, and writes nothing into the library, "which may belong to someone else."

A machine that has not cloned a library gets a warning, never a broken link. A library may also have no remote at all: [[design-thesis]]. What the skills do, step by step, is not restated here.

Source: [[engine-contributing-schedulable]] · [[engine-readme]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'anywhere and run `kb-library add <path>`. That registers it under the name its `INDEX.md` declares and' "$KB_ENGINE_ROOT/CONTRIBUTING.md"

## What do I do when something looks wrong?

- **"knowledge: engine not found — structural and secret checks skipped"**: this machine has no engine registered. Run `kb-bootstrap --apply`.
- **A commit went through unchecked**: either the case above, or `core.hooksPath` is unset in that clone. Run `kb-init` in it.
- **A skill behaves like an older version**: plugin content is loaded once, at session start. Restart the session. "Files read with Read or Bash are live, which is why the two can disagree mid-session."
- **`orphan — no router reaches it`**: the file exists but nothing lists it, so retrieval will never find it. Add its line to the right `INDEX.md`. "The golden set and `documents/` are exempt by design."
- **`raw artifact in sources/`**: a non-`.md` file is in the folder for citation stubs. Move it to `documents/` and write (or point) a stub in `sources/` at it.
- **`scope 'x' disagrees with its library`**: the fact is in the wrong home. Move it, or link to it from where it belongs rather than restating it.

Source: [[engine-contributing-schedulable]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF '**A skill behaves like an older version** — plugin content is loaded once, at session start.' "$KB_ENGINE_ROOT/CONTRIBUTING.md"

## What checks run when I commit, and in CI?

README: "Every rule in the contract that can be checked is checked by a script. Git hooks run most of them at commit time, and CI runs them again where a hook can't be skipped." The checks it lists:

- structure, routing and links within a library (`check-kb`)
- links between libraries (`check-xlinks`, at commit time only; see ADR-0010)
- scope tiers (`check-scope`)
- golden-set integrity and reachability (`check-golden`)
- scripts recorded as executable, so a clone is not left unguarded (`check-exec-bits`)
- credential-shaped strings, across full history (`check-secrets`)
- facts pasted into agent prompts instead of retrieved (`check-embedded-facts`, engine only)
- a declared tool tier for every agent, skill and command (`check-tool-scope`, engine only)
- verification stamps that claim more than the commit supports (`check-stamps`, CI only)
- a pull request that deletes a fact file, drops a `CONFLICTED` or `UNCORROBORATED` marker or widens a scope, or that changes the contract or an ADR in this repository, fails unless it carries the `owner-approved` label (`check-blast`, CI only)

CONTRIBUTING: the hooks "catch structure, never meaning". `kb-verify` and `kb-sources` are not in the hooks: "one executes commands and the other opens network connections, neither of which is something a commit should do on your behalf." `grade-routing` is not in the hooks either. `check-blast` "(CI only, on a pull request)"; "A direct push only warns."What a clean run does and does not mean is in [[guardrail-verification]].

Source: [[engine-readme]] · [[engine-contributing-schedulable]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'Every rule in the contract that can be checked is checked by a script. Git hooks run most of them' "$KB_ENGINE_ROOT/README.md"

## How do I run the engine's test suite?

`tests/run-checks [-v]` "takes about a minute and must stay green." It "breaks one thing on purpose per case and requires the check to notice." The engine's hook runs it when a commit touches `scripts/` or `tests/`, and CI runs it every time. When a check misses something, add a case with the fix: "all four checks that have ever been wrong here were wrong in a way no existing case covered." Its fixtures declare the newest contract version, so publishing a contract version means raising that line too. What the suite clears from the caller's environment is in [[guardrail-verification]].

`KB_TODAY=YYYY-MM-DD` is an injection point for the date: staleness, sweep age and the date written into a queue record or a new note treat it as today; "A malformed value is exit 2, not the real date." Timestamps such as a report's `generated` and ISO week, the schedule log and backup names "stay on the real clock." The engine's environment variables are declared in `scripts/kbengine/env.py` (`python3 -m kbengine.env` prints each variable with its default and effect); the files are at fixed paths ([[design-thesis]]).

Source: [[engine-contributing-schedulable]] · [[engine-readme]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF '`tests/run-checks [-v]` takes about a minute and' "$KB_ENGINE_ROOT/CONTRIBUTING.md"

## What are the engine repository's own hooks?

The engine's own hook is `.githooks/pre-commit`, "and git does not clone the setting." In each clone:

```bash
git config core.hooksPath .githooks
```

It runs `check-exec-bits`, `check-secrets`, `check-embedded-facts` and `check-tool-scope`, then `check-xlinks --installed` and `check-scope` over the libraries registered on your machine, and `tests/run-checks` whenever the commit touches `scripts/` or `tests/`. CI runs `check-exec-bits`, `check-secrets --all`, `check-embedded-facts`, `check-tool-scope`, `check-stamps`, `check-blast` and `tests/run-checks`.

Source: [[engine-contributing-schedulable]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'It runs `check-exec-bits`, `check-secrets`, `check-embedded-facts` and `check-tool-scope`, then' "$KB_ENGINE_ROOT/CONTRIBUTING.md"

## How do I contribute a change?

README: "Issues and pull requests are welcome. Read CONVENTIONS.md before proposing a change to the contract, and run `tests/run-checks` before proposing a change to a check." What CONTRIBUTING requires of a change to the engine:

- **A check or a contract rule changes with a case**, and the suite stays green (above).
- **A contract version is a heading.** "A new `### MAJOR.MINOR` in CONVENTIONS §14 is how an engine learns a version exists. `kb-init`'s scaffold and `kb-create-domain` write the newest, and `CHANGELOG.md` records the change under `## Unreleased`."
- **The contract and the ADRs need the owner.** "A pull request that changes `CONVENTIONS.md` or `documents/decisions/` fails `check-blast` until the repository owner labels it `owner-approved`." "Decision records are history: add a dated amendment or a new ADR rather than rewriting one."
- **A skill, agent or command declares its tools and tier** in its frontmatter. "A subagent's `tools:` is a real allowlist. A skill's or command's `allowed-tools:` only pre-approves, so its approval gates are written into its steps" (`documents/tool-scope-matrix.md`, `documents/state-change-gates.md`). Evidence for a change in behavior is kept under `documents/eval-<date>-<name>/`.
- **Releases** follow ADR-0015: semantic versions, notes from `CHANGELOG.md`, and a code freeze from two days before the release date, when `main` takes release work only ([[releases-and-contract-versioning]]).

Review: "CI checks are required on `main`, but pull-request and code-owner review wait for a second contributor, since an author can't approve their own pull request (ADR-0002)." The engine's hook and CI are in the previous section; the freeze and the reference library's shared tag are in [[releases-and-contract-versioning]].

Source: [[engine-contributing-schedulable]] · [[engine-readme]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'fails `check-blast` until the repository owner labels it `owner-approved`.' "$KB_ENGINE_ROOT/CONTRIBUTING.md"

## How does the engine resolve its root, and why?

ADR-0001 (accepted 2026-09-19, amended 2026-09-22 by ADR-0008) chose a user-level settings entry combined with a SessionStart hook that injects the resolved absolute path into session context, so no file contains a literal knowledge-base path. The amendment: "The mechanism below still holds for Claude sessions, with two changes: the variable is `KB_ENGINE_ROOT` (the engine is no longer assumed to be `<root>/knowledge`), and the hook script lives in the engine as `scripts/kb-session-start` rather than in `~/.claude/hooks/`." ADR-0008 retired `KB_GENERAL_ROOT`: the engine is "simply the directory it lives in". Git hooks, which ADR-0001 did not consider, resolve the engine through `git config --global kb.engineRoot`, and `kb-bootstrap` writes every location from one source.

Why user-level and not project-level: the root is a trust root, and "A project file can be overwritten by the repo it lives in, so a compromised or careless project could repoint the canonical knowledge every agent cites. User-level settings sit outside any repo's reach." A failure must be loud: an unset variable, or one pointing at a directory with no `knowledge/`, "produces a session-start warning and an instruction to report the root as unconfigured rather than guess."

The acceptance test of 2026-09-19 found why the hook is needed. An inline `export` in a tool call failed ("each call is a new PID; nothing persists"). `~/.bashrc` failed mid-session ("the shell snapshot is taken at session start; needs a restart"). The `~/.claude/settings.json` `env` entry passed without a restart. A sub-agent reading through the variable with Bash passed, and with the Read tool failed: "the Read tool does not expand environment variables". A hook firing at a real session start passed after a restart. Stated costs: "The mechanism is Claude Code specific — settings `env` plus a SessionStart hook. Another agent runtime would need its own equivalent, and this ADR would need revisiting."

Source: [[adr-0001-kb-root-resolution]] · [[adr-0008-machine-bootstrap]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'still holds for Claude sessions, with two changes: the variable is `KB_ENGINE_ROOT` (the engine is no' "$KB_ENGINE_ROOT/documents/decisions/0001-kb-root-resolution.md"

## What is the engine's status?

README: "This is early, working software, version 0.4.0, with one author so far." Releases are tagged and described in `CHANGELOG.md`; what a version number means is ADR-0015, how a library states which contract version it was built to is ADR-0016, and "The reference library, ai-kb-architecture, is released with the engine under the same tag" (ADR-0019) — see [[releases-and-contract-versioning]].

- **Built and tested**, as README lists it: "the contract (version 2.6), ADRs 1 to 17, the five skills, the retrieval agent, both halves of the retrieval eval, the guardrails above, and the weekly job." The same README says elsewhere that the repository holds ADR-0001 to ADR-0021.
- **Evidenced, not guaranteed**: "that an agent retrieves rather than recalls has been tested on five libraries by agents that never saw the answer key", with "The protocol changed between runs as defects in the eval itself were found". The eval's defects are in [[retrieval-eval-integrity]].
- **Next**: rolling out further domains. "No orchestrator across domain agents will be built until `kb-retrieve`'s own routing is shown to fall short (ADR-0011)." The triage step built as `kb-ask` "delivers cited answers far more often, but does not yet meet the 95% routing target at 48 libraries" ([[retrieval-agent-behavior]], [[agent-lifecycle-roles]]).
- **Known gaps** are "recorded, not hidden": review waits for a second contributor (above), and "Cross-library links are checked at commit time rather than in CI (ADR-0010)."

The license is MIT and "covers this engine only. Each domain library is its own repository, with its own terms."

Source: [[engine-readme]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'This is early, working software, version 0.4.0, with one author so far.' "$KB_ENGINE_ROOT/README.md"
