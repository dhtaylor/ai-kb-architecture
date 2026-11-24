---
name: script-reference-checks
description: Reference for ai-kb's check scripts at engine commit 303082a (check-stamps as read at v0.5.0, before it was retired) — for each check-* script, grade-eval, grade-routing, pii-scan and secret-entropy, what it is for, its usage as written, its defaults, its exit codes and what it never does; where the design is explained, it links the section rather than restating it.
memory_type: semantic
domain: knowledge-architecture
scope: general
verified: 2026-11-21
metadata:
  type: fact
  node_type: memory
  created: 2026-11-20
tags: [knowledge-architecture, scripts, reference, checks]
keywords: [script reference, exit codes, usage, check-blast, check-embedded-facts, check-exec-bits, check-golden, check-kb, check-scope, check-secrets, check-stamps (retired), check-tool-scope, check-transient, check-xlinks, grade-eval, grade-routing, pii-scan, secret-entropy, --installed, --file, --strict, --redact, --worktree, --transcripts, --labels, owner-approved, lint-allow, default path, usage error]
---
# Script reference: the checks

One section per script in the engine's `scripts/` folder, read at engine commit `5433f08`. Each gives the purpose in the header's own words, the usage line as written, defaults, exit codes and what the script never does. Why a check exists and what it found is in the design files linked from each section; this file does not repeat it. The data files `secret-patterns.txt` and `pii-allowlist.txt` are read by the scans below and are not documented here. The `kb-*` scripts and the `kbengine` package are in [[script-reference-tools]].

Where a header states no exit code, the section says so. A line marked "Observed" was seen by running the script at that commit, and is not in the header.

## check-blast

Purpose: "detect high-blast-radius changes, so they can be gated rather than merged quietly."

Usage: `check-blast <base> <head> [--labels <comma,list>]`

- `--labels` is the PR's labels, comma-separated. If `owner-approved` is among them, findings still print, but the exit code is 0 rather than 1: "detection is not suppressed, only the gate is."
- Run inside a git repository, library or engine, that already has both `<base>` and `<head>` as resolvable commits.
- Exit codes: "Exit 0 if no high-blast finding (or the owner label is present); 1 if any finding and no owner label; 2 on a usage error (bad arguments, not a git repository, base/head do not resolve, or the repo root has neither CONVENTIONS.md nor INDEX.md)."
- Never: "DETECTION ONLY. This script changes nothing." Gating on a finding is CI's job, "never this script's."

Design and the rules it enforces: already covered, [[agent-lifecycle-roles]] § Mutation governance and [[unattended-preparation]] § What stays attended whatever the schedule does.

Source: [[script-check-blast]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'Exit 0 if no high-blast finding (or the owner label is present); 1 if any finding and no owner' "$KB_ENGINE_ROOT/scripts/check-blast"

## check-embedded-facts

Purpose: "the lock on "retrieve, don't embed"." It scans agent and command files for patterns that indicate an embedded fact: absolute filesystem paths, IP addresses and host:port, fully-qualified hostnames, credential shapes (via `secret-patterns.txt`) and bare numeric constants (years excluded).

Usage: `check-embedded-facts [path ...]`

- Escape hatch: put `lint-allow` in a comment on the line, with a reason.
- Default: the header says "default: .claude/agents/, .claude/commands/". Observed: with no argument the script looks in `plugins/*/skills`, `plugins/*/agents` and `plugins/*/commands`, and a finding in `.claude/agents/` was not scanned. Which of the two is intended is an open question.
- Exit codes: "Exit 0 clean, 1 on any finding." With no file found it prints "nothing to check" and exits 0.
- Limit, stated by the header: "identifier-level facts — a table name, a column, an enum value — are not reliably machine-detectable and are NOT caught here." Those remain a human review responsibility.
- Fenced lines (inside a code fence) and lines carrying `lint-allow` are skipped.

No existing section covers this script.

Source: [[script-check-embedded-facts]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: t=$(mktemp -d); r=0; cd "$t" && mkdir -p plugins/p/agents .claude/agents && printf 'host 10.1.2.3\n' > plugins/p/agents/a.md; "$KB_ENGINE_ROOT/scripts/check-embedded-facts" >/dev/null 2>&1; [ $? -eq 1 ] || r=1; printf 'host 10.1.2.3 lint-allow: fixture\n' > plugins/p/agents/a.md; "$KB_ENGINE_ROOT/scripts/check-embedded-facts" >/dev/null 2>&1 || r=1; rm -rf plugins; printf 'host 10.1.2.3\n' > .claude/agents/a.md; "$KB_ENGINE_ROOT/scripts/check-embedded-facts" 2>&1 | grep -qF 'nothing to check' || r=1; cd /; rm -rf "$t"; exit $r

## check-exec-bits

Purpose: "guard against a bug that has now occurred four times": git records a script as `100644` where `core.fileMode` is false, and git skips a non-executable hook silently. The rule is "anything tracked that starts with a shebang must be recorded executable, wherever it lives", and anything under `.githooks/` or a `hooks/` folder.

Usage: `check-exec-bits [repo-root]   (default: .)`

- Exit codes: the header states none. Observed: 0 and "check-exec-bits: clean" when every such file is `100755`; 1 and "check-exec-bits: FAILED" with the fix `git update-index --chmod=+x <path>` otherwise; 2 when the repo-root cannot be entered.

Why it exists and how its rule was widened: already covered, [[guardrail-verification]] § Environment differences hide a defect from the authoring machine; guard by structure, not by filename and § A defect that recurs needs a check that detects it, not another fix.

Source: [[script-check-exec-bits]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: t=$(mktemp -d); r=0; export GIT_CONFIG_GLOBAL=/dev/null GIT_CONFIG_NOSYSTEM=1; git init -q "$t/r"; printf '#!/bin/sh\n' > "$t/r/x"; git -C "$t/r" add x; git -C "$t/r" update-index --chmod=-x x; "$KB_ENGINE_ROOT/scripts/check-exec-bits" "$t/r" >/dev/null 2>&1; [ $? -eq 1 ] || r=1; git -C "$t/r" update-index --chmod=+x x; "$KB_ENGINE_ROOT/scripts/check-exec-bits" "$t/r" >/dev/null 2>&1 || r=1; "$KB_ENGINE_ROOT/scripts/check-exec-bits" "$t/none" >/dev/null 2>&1; [ $? -eq 2 ] || r=1; rm -rf "$t"; exit $r

## check-golden

Purpose: "the deterministic half of the retrieval eval." It verifies that a library's golden set is a working oracle: every record is well-formed, every `expected_file` exists, every `expected_excerpt` is present in the file it names, and every `expected_file` is reachable from the library's root `INDEX.md` by router links. A cross-root case may name its file in another registered library as `<library>:<path>`. An uncorroborated case's excerpt must sit in a section marked UNCORROBORATED, and a library holding such a section must carry one.

Usage: `check-golden <library-root>`

- Exit codes: "Exit 0 clean, 1 on any defect." Observed: no argument prints `usage: check-golden <library-root>` and exits 2; a folder with no golden set exits 1.
- Never: "No LLM, no agent, no network." It "cannot tell whether an agent answers from the file or from its own memory"; that is [[retrieval-eval-integrity]]'s answer-grounding half, run by `grade-eval`.

Since v0.5.0 the record rules, the grading and the citation resolution live in engine modules, `kbengine.golden`, `kbengine.citations` and `kbengine.transcripts` (#252), which ended the load cycle in which `check-golden` and `grade-eval` each loaded the other; the five scripts print the same reports and exit with the same codes. A record's optional `expected_evidence:` is checked against the file by `check-golden` and graded by `grade-eval` (#237, [[contract-retrieval-and-verification]] § The golden set). `check-golden` reads reachability through `factfiles` (#251) and no longer loads `check-kb`.

The oracle's role and its integrity rules: already covered, [[retrieval-eval-integrity]] § An oracle must have one reader.

Source: [[script-check-golden]] · [[engine-changelog-v0.6.0]] (#237, #251, #252)
Verified: 2026-11-24 · by: agent · method: doc-review
Recheck: t=$(mktemp -d); r=0; "$KB_ENGINE_ROOT/scripts/check-golden" >/dev/null 2>&1; [ $? -eq 2 ] || r=1; "$KB_ENGINE_ROOT/scripts/check-golden" "$t" >/dev/null 2>&1; [ $? -eq 1 ] || r=1; grep -qF 'Exit 0 clean, 1 on any defect.' "$KB_ENGINE_ROOT/scripts/check-golden" || r=1; grep -qF 'expected_evidence' "$KB_ENGINE_ROOT/scripts/kbengine/golden.py" || r=1; rm -rf "$t"; exit $r

## check-kb

Purpose: "partial implementation of the CONVENTIONS §13 check table." Within a single root it covers frontmatter lint, `[[slug]]` and router links, slug uniqueness, orphans, stamp placement, Recheck syntax, the `verified:` floor, section provenance (fails from contract 2.5, warns below), `links_to` (fails from 2.6), the short lease (`unasserted_ttl`, and `agent_ttl` below contract 2.7; `agent_ttl:` fails from 2.7), `kb_contract` and contract gating.

Usage: `check-kb <knowledge-root>`

- "The root is REQUIRED." The engine repository holds no knowledge content, "so there is no sensible default and a silent one would report a clean run over zero files."
- Exit codes: "Exit 0 clean, 1 on any defect." Observed: no argument prints `Usage: check-kb <knowledge-root>` and exits 2; a folder with no markdown files exits 1.
- Exempt from the orphan rule, per §3: the root `INDEX.md`, the golden set and `documents/`.
- Elsewhere, per the header: scope values (check-scope), `[[library:slug]]` (check-xlinks), golden-set integrity (check-golden), secrets, exec bits, embedded facts.

A `[[slug]]` inside a code fence is no longer reported as a dead link, nor is a fenced link naming a path-addressed file (#257): a fence is an example. One fence definition, `factfiles.FENCE`, serves `check-kb`, `check-xlinks` and the stamp checks, and the dead-link check reads links through `check-xlinks`' reader. The change loosens a check and rejects nothing new, so no contract version changes (CONVENTIONS §14). `check-kb`'s file readers (`kb_files`, `unsourced`, `unstamped_keys`, `marked_sections`, `reachable`) moved into `factfiles` (#251); a router that is not valid UTF-8 is read with replacement characters and no longer crashes it.

How rules apply by declared contract version: already covered, [[releases-and-contract-versioning]] § A library declares its version, and rules apply by it. Orphans and the router rule: [[guardrail-verification]].

Source: [[script-check-kb]] · [[engine-changelog-v0.6.0]] (#236, #251, #257)
Verified: 2026-11-24 · by: agent · method: doc-review
Recheck: t=$(mktemp -d); r=0; "$KB_ENGINE_ROOT/scripts/check-kb" >/dev/null 2>&1; [ $? -eq 2 ] || r=1; "$KB_ENGINE_ROOT/scripts/check-kb" "$t" >/dev/null 2>&1; [ $? -eq 1 ] || r=1; grep -qF 'Exit 0 clean, 1 on any defect.' "$KB_ENGINE_ROOT/scripts/check-kb" || r=1; rm -rf "$t"; exit $r
Recheck: t=$(mktemp -d); r=0; d="$t/fxz"; mkdir -p "$d"; printf -- '---\nname: x-index\ndescription: d\nmemory_type: reference\ndomain: fxz\nscope: general\nfreshness_horizon: 90d\nverifier_budget: 5\nsweep_interval: 30d\nkb_contract: "2.7"\nlast_swept: never\n---\n# x\n\n- \133f](f.md) — f\n' > "$d/INDEX.md"; printf -- '---\nname: f\ndescription: d\nmemory_type: semantic\ndomain: fxz\nscope: general\nmetadata:\n  type: fact\n  node_type: memory\n  created: 2026-01-01\ntags: [x]\nkeywords: [x]\n---\n# f\n\n## A\n\nExample link syntax:\n\n```\n\133\133no-such-slug]]\n\133\133INDEX]]\n```\n\nSource: manual\n' > "$d/f.md"; "$KB_ENGINE_ROOT/scripts/check-kb" "$d" >/dev/null 2>&1 || r=1; printf '\nSee \133\133no-such-slug]] outside a fence.\n' >> "$d/f.md"; "$KB_ENGINE_ROOT/scripts/check-kb" "$d" >/dev/null 2>&1 && r=1; rm -rf "$t"; exit $r

## check-scope

Purpose: "the scope attribute is only a rule if something enforces it." `check-kb` proves `scope:` is present; this proves "that the value is well-formed, that it agrees with the library holding it, and that nothing reaches outside the library to fetch knowledge by relative path."

Usage: `check-scope <tree-root> [more-tree-roots...]`

- Two kinds of tree, "told apart by structure rather than by name or path": a library is its own git repository (has `.git` at its root) and a `repo:` scope there is a misfiled deployment fact. A project tree is not, is the repo-tier home, and anything but `repo:` there is a general fact misfiled into one project. "The rules are mirror images."
- A comparative product library declares `vendors:` in its root `INDEX.md`. Checked: the list names at least two vendors, each a slug, none twice; the library's scope is `product:`; the category is not one of the vendors. Not checkable: whether a slug really names a vendor, or a category is narrow enough, which is review's job.
- Exit codes: "Exit 0 clean, 1 on any defect." Observed: no argument prints `usage: check-scope <library-root> [more...]` and exits 2; a path with no `INDEX.md` exits 1 ("not a library root").
- Not covered, by the header: the CODEOWNER audit on promotion to a shared tier ("dormant — there is one owner"), and cross-library slug uniqueness, "which is obsolete".

No existing section covers this script beyond a mention in [[needs-attention]].

Source: [[script-check-scope]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: t=$(mktemp -d); r=0; "$KB_ENGINE_ROOT/scripts/check-scope" >/dev/null 2>&1; [ $? -eq 2 ] || r=1; "$KB_ENGINE_ROOT/scripts/check-scope" "$t/none" >/dev/null 2>&1; [ $? -eq 1 ] || r=1; grep -qF 'Exit 0 clean, 1 on any defect.' "$KB_ENGINE_ROOT/scripts/check-scope" || r=1; rm -rf "$t"; exit $r

## check-secrets

Purpose: "pattern scan for credential-shaped strings", in three passes per file: the patterns in `secret-patterns.txt`, then `scripts/secret-entropy`, then `scripts/pii-scan`. Any pass blocks.

Usage, as the header lists the modes, and the usage line the script prints on a bad option:

- `check-secrets` scans files staged for commit (pre-commit mode)
- `check-secrets --worktree` scans the working tree: tracked files and untracked, non-ignored ones
- `check-secrets --file PATH` scans one file, the same three passes (`kb-capture` uses it). `--file PATH --strict` switches the `documents/` exemption off.
- `check-secrets --all` scans every blob in full git history and the worktree
- `check-secrets --redact` is "a FILTER, not a gate": stdin to stdout, every line any pass flags replaced by `[redacted: secret]` or `[redacted: personal identifier]`, a count on stderr, the matched text nowhere.
- Printed on a bad option: `usage: check-secrets [--worktree|--all|--file PATH [--strict]|--redact]`

Defaults: no flag means staged files.

Exit codes: "Exit 0 clean, 1 on any hit." For `--redact`: "Exit 0 nothing redacted, 1 something was (stdout is still safe to keep), 2 the input could not be scanned (UTF-16, binary, a pass failed) and nothing was passed through, so a caller holds the input." Observed: an unknown option or a missing `--file` target exits 2.

Never: it is "not a dedicated secret scanner"; "the PII pass sees identifiers only: it cannot detect a personal NAME in prose". A UTF-16 file is reported `UNSCANNED` and blocks until converted. `lint-allow` exempts nothing under `--redact`.

The three passes, the modes, the documents/ exemption, the filter and the limits: already covered, [[secret-governance]] § What check-secrets covers: three passes, § A redaction filter keeps a secret out of an unattended model's output, § Raw artifacts archived in a library and § What check-secrets cannot see.

Source: [[script-check-secrets]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: t=$(mktemp -d); r=0; v=$(printf 'Ab3%sxyzQ9' '$'); printf 'password: %s\n' "$v" > "$t/s.txt"; printf 'a plain line\n' > "$t/c.txt"; "$KB_ENGINE_ROOT/scripts/check-secrets" --file "$t/s.txt" >/dev/null 2>&1; [ $? -eq 1 ] || r=1; "$KB_ENGINE_ROOT/scripts/check-secrets" --file "$t/c.txt" >/dev/null 2>&1 || r=1; "$KB_ENGINE_ROOT/scripts/check-secrets" --bogus >/dev/null 2>&1; [ $? -eq 2 ] || r=1; rm -rf "$t"; exit $r

## check-stamps

Retired. The script is removed from the engine after v0.5.0 (#240, ADR-0022), together with the engine's own guardrails step, the library guardrails template's "Stamp honesty" step and their cases in `run-checks`; v0.5.0 still ships it. It stopped a false `by: human` from buying a tenfold lease, and the lease now follows the stamp's evidence, so "there is nothing to stop". **A library that copied the template must delete its "Stamp honesty" step before it moves its engine pin to the first release without `scripts/check-stamps`, or its CI fails.**

What it was, as read at v0.5.0. Purpose: "refuse a commit that claims human verification while announcing that an agent wrote it." A commit whose message carries a Claude co-authorship trailer and which adds a `by: human` stamp was refused. Usage: `check-stamps [<range>]     default: HEAD~1..HEAD (HEAD alone on a first commit)`. In CI, the pushed range (before..after) was passed; "A repository with no commits yet (a library before its first commit) has nothing to check and skips." Exit codes: "Exit 0 clean, 1 on any offending commit." Observed: not a git repository, or a range that does not resolve, exited 2. Limit, stated by the header: "It cannot stop a determined author: drop the trailer and the check sees nothing." It was "a guard against the accident — an agent restamping in passing — not a control against a person who means it." It did not derive `by:` from the git committer identity, because "an agent commits under the human's own git identity". It guarded `by: human` because §7 gave a human stamp "TEN TIMES the TTL of an agent one".

The contract's statement of what `by:` can and cannot prove is §7 of CONVENTIONS.md, and the evidence tiers that replace the ten-times rule are in [[contract-provenance-and-currency]] § Evidence tiers and [[agent-lifecycle-roles]] § A stamp's lease follows its evidence, not its author.

Source: [[script-check-stamps]] (read at v0.5.0) · [[adr-0022-evidence-not-identity-sets-the-lease]] · [[engine-changelog-v0.6.0]] (#240)
Verified: 2026-11-24 · by: agent · method: doc-review
Recheck: [ ! -e "$KB_ENGINE_ROOT/scripts/check-stamps" ] && ! grep -qF 'Stamp honesty' "$KB_ENGINE_ROOT/documents/library-guardrails.yml" && ! grep -qF 'check-stamps' "$KB_ENGINE_ROOT/.github/workflows/guardrails.yml"

## check-tool-scope

Purpose: "the tier is declared and internally consistent. That is all this proves." It checks that the `tools:` of a subagent, or the `allowed-tools:` of a skill or command, is present, well-formed and within the allowance of the `tier:` the file declares.

Usage: `check-tool-scope [path ...]`

- Default: with no argument, `plugins/*/agents/*.md` (key `tools`), `plugins/*/commands/*.md` and `plugins/*/skills/*/SKILL.md` (key `allowed-tools`). With a folder argument, every `*.md` under it that is an agent, a command or a `SKILL.md`.
- The tiers and their allowances, as the header's table gives them: `retrieval` Read, Grep, Glob; `curation` Read, Grep, Glob, Write, Edit, Bash; `verification` Bash; `analysis` Read, Grep, Glob, Write; `triage` Bash, Agent.
- Exit codes: "Exit 0 clean, 1 on any finding." With no behaviour-layer file found it prints "nothing to check" and exits 0.
- What it does not prove: a subagent's `tools:` "is a real allowlist", but a skill's or command's `allowed-tools:` "does NOT restrict"; it only pre-approves for the turn. For those files the declaration "is a declaration of intent, not a fence". The check "cannot see what a skill's Bash calls actually run".

No existing section covers this script; the tier of each skill is in the skill reference files.

Source: [[script-check-tool-scope]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: t=$(mktemp -d); r=0; mkdir -p "$t/plugins/p/agents"; f="$t/plugins/p/agents/a.md"; printf -- '---\ntier: retrieval\ntools: Read, Grep\n---\n' > "$f"; "$KB_ENGINE_ROOT/scripts/check-tool-scope" "$f" >/dev/null 2>&1 || r=1; printf -- '---\ntier: retrieval\ntools: Read, Write\n---\n' > "$f"; "$KB_ENGINE_ROOT/scripts/check-tool-scope" "$f" >/dev/null 2>&1; [ $? -eq 1 ] || r=1; rm -rf "$t"; exit $r

## check-transient

Purpose: "headings shaped like a changelog or a snapshot, in a library's fact files (#136)." Two shapes: a dated heading (`YYYY-MM` or `YYYY-MM-DD` in the heading) and a ticket heading (PR, MR or WI followed by a number). Only headings are read, and a heading inside a code fence is not one.

Usage: `check-transient <library>`

- Not read: `episodic/`, `documents/`, `sources/`, every `INDEX.md`, the golden set, `needs-attention.md`, `*-open-questions.md` and `*-contradictions.md`.
- Exit codes: "Exit 0 when nothing is found, 1 when anything is, 2 on a usage error."
- Never a gate: "DETECTION ONLY. This is never a commit gate: a dated heading can be right (a dated release, a legal effective date), and only a person can say." `kb-audit` queues each hit as `transient content`.

Why: already covered, [[agent-lifecycle-roles]] § A fact file holds current state, not a changelog.

Source: [[script-check-transient]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: t=$(mktemp -d); r=0; "$KB_ENGINE_ROOT/scripts/check-transient" >/dev/null 2>&1; [ $? -eq 2 ] || r=1; grep -qF 'Exit 0 when nothing is found, 1 when anything is, 2 on a usage error.' "$KB_ENGINE_ROOT/scripts/check-transient" || r=1; rm -rf "$t"; exit $r

## check-xlinks

Purpose: "resolve [[library:slug]] references into the installed libraries." `check-kb` sees one tree and skips any prefixed link; this is "the only thing that can tell a live cross-library reference from a dead one."

Usage, as written:

```
check-xlinks --installed [tree ...]
check-xlinks <kb-root> [tree ...]
```

- `--installed` resolves against the libraries registered on this machine; "This is what the hooks run."
- `kb-root` is a directory whose library folders are the set a link can resolve into, "never a library itself: that is a usage error; pass it as a tree after --installed".
- `tree` is any further tree whose links should be resolved, typically a project `knowledge/` tree. It is "a source of links, never a target".
- Exit codes: "Exit 0 when nothing is dead, 1 otherwise, 2 on a usage error. Warnings alone do not fail."
- Form: `[[<library>:<slug>]]`, where the prefix names the library, not a tier. Absence is not a defect: a link into a library that is not installed warns; a link into an installed library naming a slug it lacks fails.

Dependency mismatches, `links_to` and read-only libraries: already covered, [[releases-and-contract-versioning]] § A library says where each library it links into comes from, and a mismatch warns. The usage error for a library folder: [[guardrail-verification]] § A check given the wrong kind of folder must refuse, not report clean.

Source: [[script-check-xlinks]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: "$KB_ENGINE_ROOT/scripts/check-xlinks" >/dev/null 2>&1; [ $? -eq 2 ] && grep -qF 'Exit 0 when nothing is dead, 1 otherwise, 2 on a usage error. Warnings alone do not fail.' "$KB_ENGINE_ROOT/scripts/check-xlinks"

## grade-eval

Purpose: "the answer-grounding half of the retrieval eval." It takes what an agent actually answered and verifies, "by grep rather than by judgement", that the expected excerpt appears in the answer and in the file the agent cited.

Usage, as written:

```
grade-eval <library-root> <answers-file>
grade-eval <library-root> --transcripts <mapping-file>
```

- Answers file: one record per line, `Q<n>|<cited-files, comma-separated>|<answer text>`. A cited file may be library-relative, library-prefixed or kb-rooted; it is resolved to a file, not matched as text.
- `--transcripts` grades subagent transcripts through the mapping file `grade-routing` reads: one record per line, `Q<n>|<transcript>`; `#` starts a comment. The answer is the message of the agent's `SubagentHandback` call, or, with none, its last text block. The cited files are the answer's last `Sources:` line.
- An uncorroborated case also requires the answer to begin "uncorroborated — single source".
- Only mapped cases are graded.
- Exit codes: "Exit 0 if every record passes, 1 otherwise, 2 on a usage error."

Why grounding is graded by grep and what the grader must and must not demand: already covered, [[retrieval-eval-integrity]] § A grader must test grounding, not formatting and § A grader must resolve identity, and demand only what the contract requires.

Source: [[script-grade-eval]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: "$KB_ENGINE_ROOT/scripts/grade-eval" >/dev/null 2>&1; [ $? -eq 2 ] && grep -qF 'Exit 0 if every record passes, 1 otherwise, 2 on a usage error.' "$KB_ENGINE_ROOT/scripts/grade-eval"

## grade-routing

Purpose: "the agent half of the routing check, graded from tool calls." It reads the tool calls in a Claude Code subagent transcript, "never what the agent said about them", and reports per golden case whether the expected file was read, which libraries had content read beyond their `INDEX.md`, and whether the golden set or an eval folder was touched.

Usage: `grade-routing <library-root> <mapping-file>`

- Mapping file: one record per line, `Q<n>|<transcript>`. `Q<n>` is the 1-based position of the case in the golden set, the same as in `grade-eval`. A path is relative to the mapping file or absolute; lines starting with `#` are comments; only mapped cases are graded.
- Exit codes: "Exit 0 if every graded case passes, 1 otherwise, 2 on a usage or format error."
- Never: "Nothing here runs an agent. A routing run is sampled and started by a person, never per commit (ADR-0013); check-golden is the per-commit tier."

The contamination, off-KB and attribution rules it applies: already covered, [[retrieval-eval-integrity]] § The agent under test must not be able to reach the oracle and § Contamination is what the contract forbids, no more and no less.

Source: [[script-grade-routing]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: "$KB_ENGINE_ROOT/scripts/grade-routing" >/dev/null 2>&1; [ $? -eq 2 ] && grep -qF 'Exit 0 if every graded case passes, 1 otherwise, 2 on a usage or format error.' "$KB_ENGINE_ROOT/scripts/grade-routing"

## pii-scan

Purpose: "pass three of check-secrets: personal identifiers in committed text."

Usage: the header gives none. Observed: `pii-scan <file-or-directory>` and `pii-scan --redact <file>`.

- Reads one file and prints `LINE:text` for each line holding a personal identifier (at most 3), exits 0 either way: "check-secrets decides what a hit means." Given a directory, it scans every file under it (skipping `.git`) and prefixes each line with the file, `PATH:LINE:text`.
- Three rules, "kept narrow because a hit blocks the commit": `email`, `account` (an uppercase NetBIOS-style domain of 2-15 chars, a backslash, a user name) and `home` (`/home/<name>/`, `/Users/<name>/`, `C:\Users\<name>\`, also under `/mnt/<drive>/`).
- Exempt: any line carrying `lint-allow: <reason>`, and any identifier matching a line of `pii-allowlist.txt` (case-insensitive, `*` wildcards).
- Exit codes: the header states exit 0 on a hit or not. It is not a gate.
- Limit: "identifiers only. It cannot see a personal NAME — that is a human call at the review gate."

The rules and placeholders: already covered, [[secret-governance]] § What check-secrets covers: three passes.

Source: [[script-pii-scan]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: t=$(mktemp -d); r=0; e=$(printf 'someone%scorp-site.net' '@'); printf 'mail %s\n' "$e" > "$t/p.txt"; printf 'a plain line\n' > "$t/c.txt"; o=$("$KB_ENGINE_ROOT/scripts/pii-scan" "$t/p.txt"); [ $? -eq 0 ] && printf '%s' "$o" | grep -q '^1:' || r=1; [ -z "$("$KB_ENGINE_ROOT/scripts/pii-scan" "$t/c.txt")" ] || r=1; rm -rf "$t"; exit $r

## secret-entropy

Purpose: "heuristic pass for credentials that have no recognisable shape."

Usage: the header gives none. Observed: `secret-entropy [--no-bare] [--redact] <file>`.

- Reads one file, prints `LINE:text` for every suspicious line, exits 0 either way: "check-secrets decides what a hit means and how many to show."
- `--no-bare` runs the labeled rule only; `check-secrets` passes it for a raw artifact archived under a library's `documents/` (issue #56). Under `--redact` it prints only the line number of each hit, and `lint-allow` exempts nothing.
- Two rules, "both deliberately narrow because this blocks a commit": `labeled` (a secret-ish label, then `=` or `:`, then a value of 8+ characters drawing on 3+ character classes) and `bare` (a token of 16+ characters with a digit and 3+ classes including a symbol, or 20+ characters of mixed-case letters and digits, with Shannon entropy >= 3.5 bits/char).
- Exit codes: none stated; 0 on a hit or not.
- Limit: "a heuristic. It will miss low-entropy or short secrets and may flag a legitimate literal; annotate the latter with `lint-allow: <reason>`."

What is and is not flagged, and dotted names: already covered, [[secret-governance]] § What check-secrets covers: three passes and § Dotted names are judged segment by segment.

Source: [[script-secret-entropy]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: t=$(mktemp -d); r=0; v=$(printf 'Ab3%sxyzQ9' '$'); printf 'password: %s\n' "$v" > "$t/s.txt"; printf 'a plain line\n' > "$t/c.txt"; o=$("$KB_ENGINE_ROOT/scripts/secret-entropy" "$t/s.txt"); [ $? -eq 0 ] && printf '%s' "$o" | grep -q '^1:' || r=1; [ -z "$("$KB_ENGINE_ROOT/scripts/secret-entropy" "$t/c.txt")" ] || r=1; rm -rf "$t"; exit $r
