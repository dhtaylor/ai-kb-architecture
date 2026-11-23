---
name: design-thesis
description: The core design thesis of a governed-KB-plus-thin-agent architecture — why knowledge and behavior are separated, the anti-pattern this rejects, and the target layering (three repository kinds: engine, domain library, project knowledge), including how a library is installed (registered per machine) and why it carries no hook, what differs for a local-only library, why the engine's own files are at fixed paths whatever XDG says, and the one reader of a library's root INDEX.md.
memory_type: semantic
domain: knowledge-architecture
scope: general
verified: 2026-10-28
metadata:
  type: fact
  node_type: memory
  created: 2026-09-19
tags: [knowledge-architecture, design, thin-agent, governed-kb]
keywords: [design thesis, knowledge as behavior, knowledge as document, anti-pattern, freshness engine, target architecture, thin agent layer, guardrails, registry, library registration, library hook, core.hooksPath, unsupported-contract, kb_contract, contract version, local-only, no remote, XDG_STATE_HOME, XDG_CONFIG_HOME, fixed paths, KB_PACKETS, KB_REPORTS, CLAUDE_CONFIG_DIR, kbengine.env, schedule.conf, first occurrence wins, frontmatter only]
---
# Design thesis: separate knowledge from behavior

## The separation principle

The core design rule for combining a governed knowledge base with an agent layer:
**separate knowledge from behavior, and let the behavior layer keep the knowledge layer honest.**

- **Knowledge** (facts: schemas, system behavior, documented bugs, topology) lives *only* in the
  governed KB — one canonical home, provenance, dedup, contradiction handling.
- **Behavior** (agents + commands) carries *only* routing, procedure, tool scoping, and
  confirmation gates. It holds **no embedded facts** — it retrieves them from the KB at runtime
  and acts.
- **The loop that makes it more than either half:** because the behavior layer can *act* (query
  live systems), it becomes the KB's freshness engine — agents re-verify facts against the live
  system and re-stamp them. The KB gains currency; the behavior layer gains a single source of
  truth.

The loop as built is in [[agent-lifecycle-roles]], "The Verifier as built". The separation principle
itself is unchanged: section 0 reads identically in the revised plan and the original.

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§0, §9.13) · [[knowledge-agent-architecture-combination-plan]] (§0, identical text)
Verified: 2026-11-12 · by: agent · method: doc-review

## The anti-pattern this rejects: knowledge-as-behavior

Two opposite ways to hold operational knowledge, compared property by property:

| Property | Knowledge-as-behavior (facts frozen in agent prompts) | Knowledge-as-document (a governed KB) |
|---|---|---|
| Single canonical fact | N copies, drift | one home |
| Provenance / auditability | none | per-section backlinks |
| Currency of facts | unmanaged | cited but unverified |
| Executable / operational | it acts | inert |
| Context economy at query time | whole prompt loads | progressive disclosure |
| Maintenance cost | scales with #agents | high ceremony |
| Team shareability | obvious, in-repo | can become bespoke/solo |
| Safety (secrets/leaks) | inlined | inert, nothing to leak |

The governed KB is the superior *knowledge* design; a behavior fleet is the superior *execution*
design. The optimum is the KB as substrate with a thin behavior layer on top of it.
**Knowledge-as-behavior is retained only as the anti-pattern to avoid** — the failure it embodies
(duplicated, unsourced, drifting facts) is exactly what the governed substrate prevents.

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§0) · [[knowledge-agent-architecture-combination-plan]] (§0, identical text)
Verified: 2026-11-12 · by: agent · method: doc-review

## Target layering

The architecture is three kinds of repository, not one tree holding both the governed substrate and
the behaviour layer:

- **Engine** — behaviour only, no domains, no facts. It holds the contract every library conforms
  to, the checking scripts, the plugins (skills, agents, commands — registered by consumers, never
  inherited by directory accident; see [[skills-agents-and-commands]]) and the decision records. Its `kb/` directory is created by the
  engine and never tracked by it.
- **Domain library** — content only, one repository per domain, portable. `INDEX.md` is the
  repository root and the domain router, with no tier chain above it. It holds gestalt-sized topic
  files with per-section provenance and currency, its own `sources/`, the `documents/` it was folded
  from, and its own golden set.
- **Project knowledge** — the repo tier, inside each project repository, travelling with the clone.

Two cross-cutting layers sit beside them. **Secrets** live in a vault or the environment only,
referenced by name, never inlined. **Guardrails** are a secret scan (committer-side and server-side)
plus an embedded-fact lint over the behaviour layer, and least-privilege enforcement for production
operations.

The rule that makes it hold: the engine holds no facts, a library holds no behaviour, and a library
is self-contained enough to be cloned onto a machine with neither an engine nor a sibling library
beside it. Anything reaching across a repository boundary uses a repo-qualified reference
(`ai-kb:CONVENTIONS.md`), never a relative path; the contract states the citation rule at `ai-kb:CONVENTIONS.md` section 6, restated in [[contract-provenance-and-currency]] under "Citing an artifact: inside the library, a stable outside page, or another repository".

Why the engine does not hold the facts: an engine that held both would assert the separation
principle in its own contract and violate it in its own layout, in the one place where the
separation most needs to be visible, because everything downstream follows its example. There was
also a practical failure: with one shared tier there was one shared `sources/`, one shared golden
set and one router chain, so a domain could not be moved, shared or cloned without dragging the rest
of the tier with it.

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§1) · [[knowledge-agent-architecture-combination-plan]] (§1, superseded) · [[adr-0007-engine-content-separation]] (amended)
Verified: 2026-10-28 · by: agent · method: doc-review

## A library is installed on a machine by being registered there

The decision that made it so:
"A library is installed on a machine when it is registered there, and only then. `<engine>/kb/`
stops being special: it is the default folder new libraries are cloned into, and nothing scans it."
The registry is a per-machine file outside every repository, `~/.config/ai-kb/libraries`. "It
describes the machine it is on, so it cannot lie about another one." A library's name is the
`domain:` value in its root `INDEX.md`, so the folder holding it can be called anything. The reason
discovery by presence was replaced: "A library that disappears fails silently. If a library folder
is moved or deleted, it is simply no longer enumerated. Retrieval then answers `fact not found` for
everything it held, which looks exactly like a correct refusal." Each registered library is
reported in one state: `ok`, `missing`, `not-a-library`, `name-mismatch` or `unsupported-contract`.
"Only `ok` libraries are served. Every other state is reported at session start, by name and path,
and never dropped silently." The fifth state is a library whose declared contract version names a
major the engine does not support, or cannot be read; see
[[releases-and-contract-versioning]]. A registration can also be marked read-only, for a consumer's
copy of a library someone else publishes ([[adr-0020-published-libraries-and-consumers]]); what that
changes is in the same file.

The cost, stated by the decision that made the split: a library has no guardrails of its own, so a
library cloned alone is unguarded until an engine is in reach. That is the price of not making a
portable library depend on an engine. What ties a library to the engine's rules, if not a pin, is in
[[releases-and-contract-versioning]].
A library carries no hook logic either. "A library is
shareable: one repository, cloned by any number of people who do not own it", so registering it,
moving it or upgrading the engine can never require a commit to it. The pre-commit hook lives in the
engine, and registering a clone sets its `core.hooksPath` to the engine's hook folder: "This is local
git configuration. Git never commits or clones it, so every clone carries its own".

The plan's earlier target was one `knowledge/` tree as the governed substrate, a thin behavior layer
of domain-worker agents with least-privilege tools routed by a thin orchestrator, and a secret scan over
both trees. It was rewritten to the three repository kinds above, and no orchestrator is built (see
[[agent-lifecycle-roles]], "Orchestrator cost bounds"). Least-privilege enforcement remains in the
guardrails list; its as-built strength is recorded under "Mutation governance" in the same file.

Source: [[adr-0014-library-registry]] · [[script-kb-libraries]] · [[adr-0016-versioned-library-contract]] · [[adr-0007-engine-content-separation]] (amended)
Verified: 2026-10-28 · by: agent · method: doc-review

## A library may be local-only

A library may be a git repository with no remote, never pushed. Nothing in the engine needs a library to have one, and nothing prompts a push. Three things differ:

- **It has no origin.** `kb-library export` lists it as a comment and it cannot be reproduced on another machine; another library links to it by its path ([[releases-and-contract-versioning]]).
- **Its only gate is the commit hook.** There is no server to run `library-guardrails.yml` or `check-blast`'s `owner-approved` label, so a `--no-verify` commit is unchecked: run the hook by hand afterwards (`<engine>/library-hooks/pre-commit` from the library root, which checks the tree as committed when it is clean).
- **A remote added later is recorded only by registering the library again** (`kb-library remove <name>`, then `kb-library add <path>`), since `kb-library add` reads the remote only when it registers a library.

Source: [[conventions-contract-versions]] · [[engine-changelog-after-v0.4.0]]
Verified: 2026-11-20 · by: agent · method: doc-review
Recheck: t=$(mktemp -d); r=0; export HOME="$t/h" GIT_CONFIG_GLOBAL="$t/gc" GIT_CONFIG_NOSYSTEM=1; mkdir -p "$HOME" "$t/fxloc"; printf -- '---\nname: x-index\ndescription: d\nmemory_type: reference\ndomain: fxloc\nscope: general\nfreshness_horizon: 90d\nverifier_budget: 5\nsweep_interval: 30d\nlast_swept: never\n---\n# x\n' > "$t/fxloc/INDEX.md"; git init -q "$t/fxloc"; k="$KB_ENGINE_ROOT/scripts/kb-library"; "$k" add "$t/fxloc" >/dev/null 2>&1 || r=1; "$k" export 2>&1 | grep '^# fxloc: no origin' >/dev/null || r=1; git -C "$t/fxloc" remote add origin https://example.invalid/fxloc.git; "$k" export 2>&1 | grep -qF 'origin = https://example.invalid/fxloc.git' && r=1; "$k" remove fxloc >/dev/null 2>&1; "$k" add "$t/fxloc" >/dev/null 2>&1; "$k" export 2>&1 | grep -qF 'origin = https://example.invalid/fxloc.git' || r=1; [ -x "$KB_ENGINE_ROOT/library-hooks/pre-commit" ] || r=1; rm -rf "$t"; exit $r

## The engine's own files are at fixed paths, whatever XDG says

The engine does not read `XDG_STATE_HOME` or `XDG_CONFIG_HOME` (ADR-0021, #205). Its own files are at:

| What | Where |
|---|---|
| Packets, reports, `schedule.log`, `schedule.lock`, backups | `~/.local/state/ai-kb/` |
| `schedule.conf`, the systemd user unit folder | `~/.config/` (`ai-kb/schedule.conf`, `systemd/user/`) |
| The library registry | `~/.config/ai-kb/libraries` (unchanged, ADR-0014) |

`KB_PACKETS` and `KB_REPORTS` stay. They are explicit, named overrides for one purpose, and a person who sets one chose it; "the trap is a variable that moves many things as a side effect." Backups, the log and the lock have no override. A person who wants state elsewhere sets `KB_PACKETS` or `KB_REPORTS` for those two, or links `~/.local/state/ai-kb` to where they want it.

The reason is a failure that was silent. A variable set only in an interactive shell's startup file is seen by the terminal and not by a git hook, cron or an IDE ([[adr-0008-machine-bootstrap]]); the registry already ignored `XDG_CONFIG_HOME` for that reason, but the engine's other files followed `XDG_STATE_HOME` or `XDG_CONFIG_HOME` in six places: packets, reports (and again in `kb-session-start`'s bash), the schedule's log and lock and its config and unit folder, and backups. With `XDG_CONFIG_HOME` set only in a terminal, `kb-schedule install` wrote `schedule.conf` under it. The cron or systemd run did not have the variable, read `~/.config/ai-kb/schedule.conf`, found nothing, and ran with preparation off and without the PATH install had saved (#165). "Nothing failed; the job did less than the person had asked for." Packets, reports and backups split the same way: a report written by the schedule was not the one the nudge named.

Three options were weighed: keep honouring XDG and tell people to set it everywhere, which the engine cannot check and which is the instruction ADR-0008 retired for `KB_ENGINE_ROOT`; honour XDG and record the resolved path where every reader can find it, which "moves the problem, and adds a file that can disagree with the variable"; and fixed paths with an explicit override where one is wanted, which was chosen.

Git's own use of `XDG_CONFIG_HOME` is out of scope: `kb-bootstrap` looks for `$XDG_CONFIG_HOME/git/config` only to find the file `git config --global` writes when `~/.gitconfig` is absent. That is git's convention, and the engine is "predicting git, not choosing a location."

Source: [[adr-0021-engine-state-at-fixed-paths]] · [[engine-changelog-after-v0.4.0]]
Verified: 2026-11-20 · by: agent · method: doc-review
Recheck: t=$(mktemp -d); r=0; p() { env -u KB_PACKETS -u KB_REPORTS HOME="$t" XDG_STATE_HOME="$t/x" XDG_CONFIG_HOME="$t/c" "$@" PYTHONPATH="$KB_ENGINE_ROOT/scripts" python3 -m kbengine.env --path "$PATH_"; }; chk() { [ "$(PATH_=$1 env -u KB_PACKETS -u KB_REPORTS HOME="$t" XDG_STATE_HOME="$t/x" XDG_CONFIG_HOME="$t/c" PYTHONPATH="$KB_ENGINE_ROOT/scripts" python3 -m kbengine.env --path "$1")" = "$2" ]; }; chk packets "$t/.local/state/ai-kb/packets" || r=1; chk reports "$t/.local/state/ai-kb/reports" || r=1; chk config "$t/.config" || r=1; chk registry "$t/.config/ai-kb/libraries" || r=1; [ "$(KB_PACKETS="$t/pk" HOME="$t" PYTHONPATH="$KB_ENGINE_ROOT/scripts" python3 -m kbengine.env --path packets)" = "$t/pk" ] || r=1; rm -rf "$t"; exit $r

### A person who had set either variable must move their files

If you had either variable set, the engine no longer reads the old folders: move your packets, reports, log and backups from `$XDG_STATE_HOME/ai-kb/` to `~/.local/state/ai-kb/`, and run `kb-schedule install` again (its conf and units were under `$XDG_CONFIG_HOME`). Nothing is deleted. The files under the old state folder and the old config folder are not found by the engine until they are moved.

Source: [[engine-changelog-after-v0.4.0]] · [[adr-0021-engine-state-at-fixed-paths]]
Verified: 2026-11-20 · by: agent · method: doc-review
Recheck: t=$(mktemp -d); r=0; mkdir -p "$t/x/ai-kb/packets/fx"; printf -- '---\npacket: kb-audit\nlibrary: fx\nstatus: pending\n---\n' > "$t/x/ai-kb/packets/fx/p.md"; e() { env -u KB_PACKETS -u KB_REPORTS HOME="$t" XDG_STATE_HOME="$t/x" XDG_CONFIG_HOME="$t/c" "$@"; }; e "$KB_ENGINE_ROOT/scripts/kb-apply" list > "$t/o" 2>&1; grep -qF "no packets waiting in $t/.local/state/ai-kb/packets" "$t/o" || r=1; e KB_PACKETS="$t/x/ai-kb/packets" "$KB_ENGINE_ROOT/scripts/kb-apply" list > "$t/k" 2>&1; grep -qF 'fx' "$t/k" && ! grep -qF 'no packets waiting' "$t/k" || r=1; [ -f "$t/x/ai-kb/packets/fx/p.md" ] || r=1; rm -rf "$t"; exit $r

### One module declares every variable the engine reads

`kbengine.env` declares every variable the engine reads, with its default and effect, and resolves its locations in one place (#205). It replaces five copies of the state-folder rule and two of the `KB_CLAUDE` fallback. `python3 -m kbengine.env --names` lists the variables. Python readers call it. `kb-session-start` and `kb-bootstrap` are bash and resolve the reports folder, the backup folder and the registry themselves. Neither script reads `XDG_STATE_HOME` any longer.

Source: [[adr-0021-engine-state-at-fixed-paths]] · [[engine-changelog-after-v0.4.0]]
Verified: 2026-11-20 · by: agent · method: doc-review
Recheck: r=0; o=$(PYTHONPATH="$KB_ENGINE_ROOT/scripts" python3 -m kbengine.env --names) || r=1; for v in KB_PACKETS KB_REPORTS KB_TODAY CLAUDE_CONFIG_DIR; do printf '%s\n' "$o" | grep -qx "$v" || r=1; done; printf '%s\n' "$o" | grep -q '^XDG' && r=1; exit $r

### CLAUDE_CONFIG_DIR is honoured where Claude Code's own files are read

`kb-telemetry` and `kb-eval-sample` honour `CLAUDE_CONFIG_DIR` (#205). They read transcripts from `~/.claude/projects` only, though Claude Code writes them under that variable's folder when it is set, and `kb-bootstrap` already followed it. `kb-report`'s count of fact-not-found runs reads the same folder. `--projects` still wins. `kb-schedule install` now saves `CLAUDE_CONFIG_DIR` in `schedule.conf`, and `run` restores it, as it does PATH; reinstall to record it. The reason given for honouring this one: Claude Code owns the variable, and the engine follows where Claude Code puts its transcripts.

Source: [[adr-0021-engine-state-at-fixed-paths]] · [[engine-changelog-after-v0.4.0]]
Verified: 2026-11-20 · by: agent · method: doc-review
Recheck: t=$(mktemp -d); [ "$(CLAUDE_CONFIG_DIR="$t/cc" HOME="$t" PYTHONPATH="$KB_ENGINE_ROOT/scripts" python3 -m kbengine.env --path claude-projects)" = "$t/cc/projects" ]; r=$?; grep -qF 'claude_config_dir=' "$KB_ENGINE_ROOT/scripts/kb-schedule" || r=1; rm -rf "$t"; exit $r

## One reader serves a library's root INDEX.md

`kb-due`, `kb-verify`, `kb-prepare` and the queue module read a library's root `INDEX.md` fields from its frontmatter only (#203). They searched the whole file, so a `last_swept:`, `archived:`, `freshness_horizon:`, `verifier_budget:`, `agent_ttl:` or `domain:` line in the body, which is prose, could be read as a declaration when the frontmatter had none. One reader of that file now serves every script (`kbengine/library.py`), as `kbengine/contract.py` does for the contract versions.

- **One rule reads every value.** A trailing ` # comment` after a value is no part of the value, and a quoted value is read without its quotes: drop a ` #` comment outside quotes, then one pair of matching quotes. Before it, `check-kb` failed `name: "widgets"  # note` against the filename `widgets`, and `kb-verify` ignored a quoted `freshness_horizon`. It only relaxes what a library may write, so it needs no contract version.
- **On a repeated key the first occurrence wins.** `check-kb` and `kb-catalog` kept the last.
- **`kb-libraries` treats a `domain:` value outside `[\w-]+` (such as `foo#bar`) as a name mismatch.**

Source: [[engine-changelog-after-v0.4.0]]
Verified: 2026-11-20 · by: agent · method: doc-review
Recheck: python3 -I -B -c 'import sys; sys.path.insert(0, sys.argv[1]); from kbengine import library as L; fm = L.read("---\nname: a\nname: b\ndomain: \"widgets\"  # note\nscope: general  # c\nlast_swept: 2026-01-01\n---\nfreshness_horizon: 7d\n"); sys.exit(0 if (fm.get("name"), fm.get("domain"), fm.get("scope"), fm.get("last_swept"), fm.get("freshness_horizon")) == ("a", "widgets", "general", "2026-01-01", None) else 1)' "$KB_ENGINE_ROOT/scripts" && (t=$(mktemp -d); r=0; d="$t/fxb"; mkdir -p "$d"; printf -- '---\nname: x-index\ndescription: d\nmemory_type: reference\ndomain: fxb\nscope: general\nfreshness_horizon: 90d\nverifier_budget: 5\nsweep_interval: 30d\n---\n# x\n\nlast_swept: 2099-01-01\n' > "$d/INDEX.md"; "$KB_ENGINE_ROOT/scripts/kb-due" --quiet "$d" 2>&1 | grep -qF 'fxb' || r=1; rm -rf "$t"; exit $r)

## How the separation holds depends on the runtime

The claim that behaviour "retrieves at
runtime" is about where facts live, not about when the behaviour itself is loaded — and those differ:
in Claude Code, plugin behaviour is loaded once at session start while facts are read live per query
(recorded in a separate claude-code-runtime library, not published with this one). The separation holds; the two halves simply refresh on
different clocks.

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§1) · [[knowledge-agent-architecture-combination-plan]] (§1, superseded) · [[adr-0007-engine-content-separation]] (amended) · [[adr-0014-library-registry]] · [[script-kb-libraries]] · [[adr-0016-versioned-library-contract]]
Verified: 2026-10-28 · by: agent · method: doc-review
