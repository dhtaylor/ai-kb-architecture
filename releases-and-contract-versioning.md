---
name: releases-and-contract-versioning
description: How the engine is versioned and released (semantic versioning, the two-day freeze, what a library release records, and why the reference library takes the engine's release tag) and why the library contract has its own MAJOR.MINOR version that a library declares, how rules apply by that version, what a library behind, ahead or on an unsupported major is told, what each contract version adds, why pinning the engine is the library owner's business, and how a library someone else publishes is installed: read-only (optionally pinned to a release tag with `--ref`), with `links_to` naming where each library it links into comes from, and a same-named library from elsewhere reported as a dependency mismatch.
memory_type: semantic
domain: knowledge-architecture
scope: general
verified: 2026-10-28
metadata:
  type: fact
  node_type: memory
  created: 2026-10-28
tags: [knowledge-architecture, releases, contract, versioning]
keywords: [release, semantic versioning, minor bump, patch, code freeze, freeze, pinning, kb_contract, contract version, MAJOR.MINOR, declared version, behind, ahead, unsupported major, unsupported-contract, last two majors, conformance suite, Not covered here, Recheck (attended), sources, documents, migration notes, reference library, same tag, release package, release day, ADR-0015, ADR-0016, ADR-0019, ADR-0020, Liveness manual, unsourced section, links_to, origin, dependency mismatch, read-only, consumer, published library, kb-library add, --read-only, --ref, aliasing]
---
# Releases and contract versioning

An engine that checks other people's libraries has two things to version: its own code, and the statement of what a library must hold. They change for different reasons, and the engine's design keeps them apart.

## The engine uses semantic versioning

A release is a git tag `vMAJOR.MINOR.PATCH` on `main`, with a release on the hosting platform whose notes come from `CHANGELOG.md`. "While the major version is 0, any change to the contract (`CONVENTIONS.md`), to what a check accepts or rejects, or to a decision record is a **minor** bump. A fix that changes no rule a library must meet is a **patch**." Both plugins' manifests carry the engine's release version, so a plugin says which release it came from. Publishing a new contract minor is an engine minor, and a new contract major is an engine major once the engine is past 0.x.

The first release was numbered 0.2.0 and not 0.1.0, because `0.1.0` had sat in the manifests and the README since the first commit and named every unreleased state of the engine. Tagging a release with that number would have given two different things one name.

Source: [[adr-0015-releases-and-pinning]] · [[adr-0016-versioned-library-contract]]
Verified: 2026-10-28 · by: agent · method: doc-review

## A release is preceded by a two-day freeze

From two days before a release date, `main` takes release work only: the changelog's release section, version numbers, the fresh-clone check and the tag. No code, check or contract change merges in that window. A defect the release work finds in the window is the owner's call: fix it and move the release date, or release without the fix and fix it in the next release.

The reason is that what is tagged should be what was checked. The fresh-clone check and the notes describe a `main` that no longer moves under them.

Source: [[adr-0015-releases-and-pinning]]
Verified: 2026-10-28 · by: agent · method: doc-review

## A library release records what it was verified against, and a correction does not wait for one

A library is content, so a correction propagates when it lands, not at the next release. A library tag marks a known-good point, nothing more. A library's release notes name the contract version it conforms to, not an engine version. The engine's reference library is the one exception: it takes the engine's release tag, below ([[adr-0019-engine-and-reference-library-release-together]]).

Whether a library's CI pins the engine, to what, and when it moves is the library owner's decision. The engine neither requires nor tracks it, and an engine release moves no library's pin. The engine's CI template (`documents/library-guardrails.yml`) stays as an example and clones a release tag. ADR-0016 calls pinning a release tag a sensible default for unattended use, and whether to pin the library owner's choice.

This replaced an earlier rule that every library's CI pin the engine to a release tag, with each release moving the pins. That rule assumed the engine's maintainer knows every library. The engine is meant for wide adoption, so the expected case is libraries owned by people the maintainer will never hear of, and the earlier decision's own review trigger, a library maintained by someone else, is the normal state.

Source: [[adr-0015-releases-and-pinning]] · [[adr-0016-versioned-library-contract]] · [[engine-changelog-v0.3.0]]
Verified: 2026-10-28 · by: agent · method: doc-review

## The reference library takes the engine's release tag

This library is the one exception to a library versioning itself. It is, in essence, the engine's detailed documentation: how the engine works and why. It is folded from the engine before each release, so it describes one engine version, and it is tied to the engine in a way no other library is. Every other library is released separately and versions itself.

Each engine release `vX.Y.Z` tags this library `vX.Y.Z` on the same day, at the library commit verified against that engine. The rule starts at v0.4.0 for both, so this library goes from v0.2.0 to v0.4.0, and no library v0.3.0 exists. Before it, the two never shared a number: engine v0.2.0 with library v0.1.0 on 2026-10-16, and engine v0.3.0 with library v0.2.0 on 2026-10-31.

- **Published tags stay as they are.** Library v0.1.0 and v0.2.0 are not moved or renamed, because someone may have pinned them.
- **The notes still name the engine.** This library's release notes say "verified against ai-kb vX.Y.Z" and name the library contract it declares. "A matching number does not replace that proof."
- **A fix to this library alone waits for the next release.** It lands on `main` as usual, but it gets no tag of its own. An engine patch release `vX.Y.Z+1` tags this library too, even if the library has not changed.

On release day the two-day freeze covers both repositories. The release script stops at the first step that fails:

1. Preflight: both `main` branches are still the commits the fresh-clone check passed, neither repository has tag `vX.Y.Z` yet, both working trees are clean, and the release pull request is green with the checked commit as its head.
2. Merge the engine's release pull request, and confirm the merged tree matches the checked head.
3. Tag the engine `vX.Y.Z`, and publish its release from the changelog's `vX.Y.Z` section.
4. Tag this library `vX.Y.Z` at its verified commit, with the message "verified against ai-kb vX.Y.Z", and publish its release from its library notes.
5. Cross-link the two releases: the engine's notes name the library release, and the library's notes name the engine release.

The decision states its costs. The library's number skips v0.3.0, which looks like a missing release to someone reading its tag list, and its v0.4.0 notes say why. A library-only fix has no release of its own. And the library's version number no longer says how much the library changed, so its changelog or release notes have to. It is to be revisited if this library ever needs a release the engine does not, or if a second library joins the release package.

Source: [[adr-0019-engine-and-reference-library-release-together]]
Verified: 2026-11-06 · by: agent · method: doc-review

## The contract has its own version, so the engine can tighten a rule without breaking a library

The library contract carries a version `MAJOR.MINOR`, independent of the engine's. It changes only when what a library must or may hold changes. An engine fix that changes no rule, and an editorial change to the contract's text, leave it alone.

- **Minor:** adds a field, a warning, or a rejection that applies only to a library declaring that version or later. A library valid under an earlier minor of the same major stays valid and stays served.
- **Major:** changes what something a library already holds means (a marker, a field, a layout), so an engine that does not know the major could misread the library. Each major ships with migration notes from the one before it.

The case that forced it: a new rejection of a raw file under `sources/` was merged and unreleased. Released as it stood, it would have failed libraries nobody had told, on the day their user upgraded the engine. The decision's rule is that a library built correctly to the rules of its day must not become unusable because the engine released again.

Three options were weighed. The first, chosen: the library's declared contract version decides which rules apply to it. A new rejection applies to libraries declaring the version that introduced it and only warns for the rest, and a library adopts it when its owner raises the declared version. The second, rejected: new rejections wait for a major version. It is simpler to implement, but rules would change in rare, large steps, each a migration for every library at once. The third, rejected: keep per-release pinning, which does not scale past the libraries one person can move and gives a stranger nothing to build to.

The contract is meant to be a spec a stranger can build to, complete without the engine's code, so that a person who has read only it can build a library the engine accepts.

Source: [[adr-0016-versioned-library-contract]] · [[tests-run-checks]]
Verified: 2026-10-28 · by: agent · method: doc-review

## A library declares its version, and rules apply by it

A library declares the version it was built to as `kb_contract:` in its root `INDEX.md`. It is quoted, always, because a YAML reader turns a bare `2.10` into the number 2.1. `check-kb` fails a bare or malformed value. A library that declares nothing is read as 2.0, so no library that predates the field had to change.

Rules apply by the declared version. A rejection introduced in 2.1 fails a library declaring 2.1 or later. A library declaring 2.0 gets a warning that names the rule and the version that brings it in, and its check still passes. So upgrading the engine never fails a library that was valid when it was built.

What a library is told depends on where its version sits:

- **Behind** the newest version the engine knows: told by `check-kb` and by the session-start nudge which library, the version it declares, the version the engine knows, and what the newer versions add. It is information, not a failure, and nothing is blocked.
- **Ahead** by a minor: checked by the newest rules the engine knows, with a note that the engine should be upgraded to check the rest.
- **On a major the engine does not support, or with a version that cannot be read:** not served. The registry reports it in the state `unsupported-contract`, and `check-kb` fails it with the versions the engine does support.

An engine supports every minor of the current major and of the one before it, so the window spans years, not releases. When a new major is published the engine drops the major two behind it. ADR-0016 specifies that a library on the previous major is warned ahead of time that its major leaves support when the next is published. No source read records that warning as built; see [[knowledge-architecture-open-questions]].

A library on an unsupported or newer major is refused rather than served with a warning because its markers may mean something this engine does not read. Serving it "could serve a disputed fact as settled, which the retrieval contract exists to prevent."

Source: [[adr-0016-versioned-library-contract]] · [[script-check-kb]] · [[script-kb-libraries]] · [[conventions-contract-versions]] · [[2026-10-26-kb-retrieve-routing-retest-after-grader-and-scope-fixes]]
Verified: 2026-10-28 · by: agent · method: doc-review
Recheck: s="$KB_ENGINE_ROOT/scripts"; t=$(mktemp -d); r=0; mk() { d="$t/$1"; mkdir -p "$d/sources"; printf -- '---\nname: x-index\ndescription: d\nmemory_type: reference\ndomain: %s\nscope: general\nfreshness_horizon: 90d\nverifier_budget: 5\nsweep_interval: 30d\n%s\nlast_swept: never\n---\n# x\n' "$1" "$2" > "$d/INDEX.md"; [ -z "$3" ] || printf 'CREATE TABLE t (id int);\n' > "$d/sources/t.sql"; }; mk absent "agent_ttl: 30d" raw; mk v20 'kb_contract: "2.0"' raw; mk v21 'kb_contract: "2.1"' raw; mk v29 'kb_contract: "2.9"'; mk bare "kb_contract: 2.1"; mk v30 'kb_contract: "3.0"'; "$s/check-kb" "$t/absent" > "$t/a" 2>&1 && grep -qF 'built to contract 2.0' "$t/a" && grep -qF 'fails from contract 2.1' "$t/a" || r=1; "$s/check-kb" "$t/v20" > /dev/null 2>&1 || r=1; "$s/check-kb" "$t/v21" > "$t/b" 2>&1; [ $? -eq 1 ] && grep -qF 'raw artifact' "$t/b" || r=1; "$s/check-kb" "$t/v29" > "$t/c" 2>&1 && grep -qF 'upgrade the engine' "$t/c" || r=1; "$s/check-kb" "$t/bare" > "$t/d" 2>&1; [ $? -eq 1 ] && grep -qF 'must be quoted' "$t/d" || r=1; "$s/check-kb" "$t/v30" > "$t/e" 2>&1; [ $? -eq 1 ] && grep -qF 'not supported by this engine' "$t/e" || r=1; rm -rf "$t"; exit $r

## What each contract version adds

The versions an engine knows are the `### X.Y` headings of the contract's section 14, and what each adds is its first paragraph. The text a library's owner is shown when the library is behind is read from there.

- **2.0** is the baseline: the rules a library met under the engine's first release. Every library that predates `kb_contract:` was built to it.
- **2.1** makes `sources/` hold only `.md` citation stubs. `check-kb` fails a raw artifact there, which belongs under `documents/` with a stub that cites it. Before the rule, `check-kb` read only `.md` files, so a raw file under `sources/` passed silently and its owner never learned it was in the wrong place. Migrating from 2.0 means moving any non-`.md` file under `sources/` to `documents/`.
- **2.2** adds `Recheck (attended):` for an assertion that needs a login, a credential or a live system. How a scheduled `kb-verify --yes --unattended` treats it is in [[agent-lifecycle-roles]]. Migrating from 2.1 means marking any assertion that needs a person and raising the declaration. The reason is in [[unattended-preparation]].
- **2.3** lets a root `INDEX.md` carry a `Not covered here:` line naming the neighboring topics the library does not hold and, where known, the library that does. `check-kb` fails a line with nothing after the colon, from this version, and warns below it. Nothing is required to migrate. How retrieval uses the line is in [[retrieval-agent-behavior]].

Source: [[conventions-contract-versions]] · [[script-check-kb]] · [[tests-run-checks]]
Verified: 2026-10-28 · by: agent · method: doc-review
Recheck: s="$KB_ENGINE_ROOT/scripts"; t=$(mktemp -d); r=0; mk() { d="$t/$1"; mkdir -p "$d"; printf -- '---\nname: x-index\ndescription: d\nmemory_type: reference\ndomain: %s\nscope: general\nfreshness_horizon: 90d\nverifier_budget: 5\nsweep_interval: 30d\n%s\nlast_swept: never\n---\n# x\n\n%s\n' "$1" "$2" "$3" > "$d/INDEX.md"; }; mk old "agent_ttl: 30d" ""; mk e23 'kb_contract: "2.3"' "Not covered here:"; mk e22 'kb_contract: "2.2"' "Not covered here:"; mk att 'kb_contract: "2.3"' ""; printf -- '---\nname: f\ndescription: d\nmemory_type: semantic\ndomain: att\nscope: general\nverified: 2020-01-01\nmetadata:\n  type: fact\n  node_type: memory\n  created: 2020-01-01\ntags: [x]\nkeywords: [x]\n---\n# F\n\n## A\n\nfact\n\nSource: manual\nVerified: 2020-01-01 · by: agent · method: manual\nRecheck (attended): false\n' > "$t/att/f.md"; "$s/check-kb" "$t/old" > "$t/o" 2>&1 && grep -qF '2.1: `sources/` holds only `.md` citation stubs' "$t/o" && grep -qF '2.2: `Recheck (attended):` marks an assertion' "$t/o" && grep -qF '2.3: A root `INDEX.md` may carry a `Not covered here:`' "$t/o" || r=1; "$s/check-kb" "$t/e23" > "$t/a" 2>&1; [ $? -eq 1 ] && grep -qF 'names nothing' "$t/a" || r=1; "$s/check-kb" "$t/e22" > "$t/b" 2>&1 && grep -qF 'fails from contract 2.3' "$t/b" || r=1; "$s/kb-verify" "$t/att" --yes --unattended > "$t/v" 2>&1 && grep -qF '1 attended assertion(s) skipped' "$t/v" && ! grep -qF 'FAIL' "$t/v" || r=1; "$s/kb-verify" "$t/att" --yes > "$t/w" 2>&1; grep -qF 'FAIL f.md' "$t/w" || r=1; rm -rf "$t"; exit $r

### Versions 2.4 to 2.6

- **2.4** lets a source stub carry `- **Liveness:** manual` for a source a script cannot check: `kb-sources` does not fetch it, and the weekly report lists it once per sweep interval for a person to check. `check-kb` fails a `Liveness:` value other than `manual`, from this version. Nothing is required to migrate.
- **2.5** requires a `Source:` line on every section with content in a fact file. The intro under the `#` title, a heading with only subheadings under it, and a `CONFLICTED` section are exempt. `check-kb` fails a section with no `Source:` line, from this version, and `kb-due` counts them. Migrating from 2.4: `check-kb` warns about each unsourced section below 2.5, and the owner gives each one its source, moves a fact out of the intro under a heading, or removes what nobody can source.
- **2.6** requires a tree that links across libraries to list, under `links_to:` in its root `INDEX.md`, the origin of each library its `[[library:slug]]` links name: the git remote it was linked against. `check-kb` fails a link whose library is not listed, from this version, and warns about an entry no link uses. A link inside an inline code span explains the form and needs no entry. Migrating from 2.5 means adding each linked library under `links_to:` with the git remote it was cloned from; "a library with no cross-library links changes only the declaration." Why, and what the engine does with the list, is below.

Source: [[conventions-contract-versions]]
Verified: 2026-11-09 · by: agent · method: doc-review
Recheck: s="$KB_ENGINE_ROOT/scripts"; t=$(mktemp -d); r=0; mk() { d="$t/$1"; mkdir -p "$d"; printf -- '---\nname: x-index\ndescription: d\nmemory_type: reference\ndomain: %s\nscope: general\nfreshness_horizon: 90d\nverifier_budget: 5\nsweep_interval: 30d\n%b\nlast_swept: never\n---\n# x\n\n- \133\133%s:y]] link\n' "$1" "$2" "other" > "$d/INDEX.md"; }; mk v25 'kb_contract: "2.5"'; mk v26 'kb_contract: "2.6"'; mk ok 'kb_contract: "2.6"\nlinks_to:\n  other: https://example.com/x/other.git'; "$s/check-kb" "$t/v25" > "$t/a" 2>&1 && grep -qF 'links_to does not name' "$t/a" && grep -qF 'fails from contract 2.6' "$t/a" || r=1; "$s/check-kb" "$t/v26" > "$t/b" 2>&1; [ $? -eq 1 ] && grep -qF 'links_to does not name' "$t/b" || r=1; "$s/check-kb" "$t/ok" > /dev/null 2>&1 || r=1; rm -rf "$t"; exit $r

## What the design costs

The decision states its own costs. Every new rejection carries a version check, and the engine keeps two majors' rules, which dropping the older major when a new one ships bounds. A library whose owner ignores two majors' warnings stops being served, which falls only on a library nobody has updated across two contract majors. The checks can no longer simply apply "the rules": each rule needs the version that introduced it, and the tests cover each rule both below and at its version. A library behind the contract is reminded at every session start until it moves, deliberately, and that may be noisy for an owner who has decided to stay.

The decision's delivery plan also named a conformance suite per contract version and a split of the contract so it holds only the library-facing spec. No source read records either as built; see [[knowledge-architecture-open-questions]].

Source: [[adr-0016-versioned-library-contract]]
Verified: 2026-10-28 · by: agent · method: doc-review

## A library someone else publishes: three things went wrong

ADR-0015 said to revisit its decisions "when a library is maintained by someone who is not the engine's maintainer". ADR-0020 records that this has happened: a library someone else publishes is installed with `kb-library add` on a machine that already holds a library of its own under a name the published one links into. It names three problems.

1. **"A link names a library, and a name means something only on one machine."** A link was written against the publisher's library of that name, but the registry makes the registered name the only key and lets a name be registered once, so on the consumer's machine it resolves to the consumer's own library. A slug missing there is a dead link, and `check-xlinks` fails the commit. The consumer cannot fix the published library, and a "fix" in their own library would hide the problem. Nothing told "the library is absent" (a warning) from "a different library holds this name" (a failure, reported as an ordinary dead link).
2. **"A library states no dependencies."** Its release did not say which other libraries it links into, or from where, so `kb-library add` could not warn before anything broke.
3. **"Every registered library is treated as the machine's own."** The nudge, `kb-audit`, `kb-distill`, `kb-capture`, the weekly job and the commit hook all assumed the machine curates every library it has. A consumer of a published library wants it read-only: what they learn about it "belongs upstream as an issue, not in local commits that drift from the release they installed."

Three options for identity were weighed. Globally unique names (`owner/name`) were rejected: every `[[library:slug]]` link in every library would change, "a major contract version for a problem only some machines have." An alias on registration was deferred: the consumer could then hold both libraries, but the rule that the registered name is the name a library declares would end, and every reader of the registry would need to learn the mapping. Chosen: a declared origin for each library a library links into, compared with the origin the registry already records. Links are unchanged, and a mismatch can be named for what it is. A block that also listed every slug relied on was rejected, because "the links already say which slugs a library relies on, and a second list would drift from them."

Source: [[adr-0020-published-libraries-and-consumers]]
Verified: 2026-11-09 · by: agent · method: doc-review

## A library says where each library it links into comes from, and a mismatch warns

From contract 2.6 a library that links across libraries lists each one under `links_to:` in its root `INDEX.md`, with the origin the author linked against (above). The slugs are not listed: they are read from the links.

Origins are compared by host and path. Scheme, user, case of the host and a trailing `.git` don't matter, so the `https://` and `git@` forms of one repository are the same origin. `kb-library import` uses the same comparison; it used to tell those forms apart.

With `--installed`, `check-xlinks` reads a library's `links_to` entry for each library it links into, and the origin the registry records for that library:

- **registered, same origin:** resolved as before; a missing slug is a dead link, and fails.
- **registered, different origin:** a **dependency mismatch**. The links into it are not resolved, and it warns, naming both origins and the slugs. It does not fail.
- **registered with no origin recorded:** resolved as before, plus a note that the origin cannot be confirmed.
- **not registered:** absent, which warns, naming the origin to `kb-library add`.
- **no `links_to` declared (below 2.6):** as before.

"A mismatch warns because the person who sees it can't fix it. They can't edit a published library, and editing their own would make the warning go away without changing what is wrong."

`kb-library add` warns, never refuses, both ways: when the new library's dependency here is from elsewhere or absent, with the number of links affected, and when a library already registered expects the new one from elsewhere. Like the overlap warning, it is advice and never changes the exit status.

Source: [[adr-0020-published-libraries-and-consumers]] · [[engine-changelog-v0.4.0]] · [[conventions-contract-versions]]
Verified: 2026-11-09 · by: agent · method: doc-review

## A consumer's copy is registered read-only: served, never curated here

`kb-library add <path | git-url> --read-only` records the library as read-only in the registry. Re-adding the same path without the flag makes it curated again. A read-only library "is **served exactly as any other**", by `kb-ask` and `kb-retrieve`. What changes is that the machine does not curate it:

- **The engine's library hook refuses commits in it**, naming the origin to file the correction with and the command that makes the library curated here. `--no-verify` still gets past it: "governance, not enforcement".
- **Nothing that writes into a library runs on it.** `kb-verify --yes/--apply`, `kb-sources --yes`, `kb-prepare` and `kb-capture --into` refuse it, and `kb-watch` skips its sources. A dry run of `kb-verify` or `kb-sources`, which writes nothing, still works. The reason given for refusing verification: "The publisher verifies their library; a consumer verifying it would change a copy no one else reads."
- **Nothing asks the machine to curate it.** The weekly job, the nudge and `kb-report` leave it out. Plain `kb-due` lists it as `read-only`.
- **The curation skills refuse to write to it** (`kb-update-domain`, `kb-distill`, `kb-organize-domain`, `kb-audit`, `/kb-capture`) and say to file the finding with its origin. Session start marks each read-only library `(read-only)`, which is how a skill knows. A skill never files the issue itself: "posting to another person's tracker is a person's decision."
- **Its outbound links never fail the machine's commits.** A dead link in a read-only library warns instead of failing. "Its publisher's CI is where it fails."

A read-only flag on the registration was chosen over making files read-only on disk, because a file mode does not survive a `/mnt/c` mount, and it would stop `git pull` too. `kb-library list` shows the mode, and `kb-libraries --read-only` lists each read-only library with its origin.

Source: [[adr-0020-published-libraries-and-consumers]] · [[engine-changelog-v0.4.0]]
Verified: 2026-11-09 · by: agent · method: doc-review

## A read-only copy can be pinned to a release tag

`kb-library add <git-url> --read-only --ref <tag>` clones at the tag and records it as `ref` in the registry. It refuses, and clones nothing, if the remote has no such tag. `--ref` goes with `--read-only`; without it, `add` is a usage error. The pin is a registry fact about a consumer's copy, so `export` and `import` do not carry the ref.

- **The registry shows the pin.** `kb-library list` shows `read-only @<tag>`, and says `(checkout moved: HEAD <sha>)` on the row if the clone is no longer at that tag. `kb-libraries` reads `ref` too.
- **Moving to a newer release is a person's step.** Check the tag out in the clone, then run `kb-library add <path> --read-only --ref <tag>`. It checks HEAD is at the tag and changes nothing if not. Re-adding without `--ref` clears the pin.

Source: [[engine-changelog-v0.4.0]] · [[script-kb-library]] · [[adr-0020-published-libraries-and-consumers]]
Verified: 2026-11-11 · by: agent · method: doc-review

## What ADR-0020 costs and leaves open

The decision delivers in three steps, all in v0.4.0: the read-only role, then `links_to` with dependency mismatches, then `--ref` pinning (above).

The stated costs: contract 2.6 means a pull request in every library that links across libraries, to declare `links_to`, and one in every library to declare 2.6. `links_to` is a second place an origin is written, so if a dependency moves its repository, every library linking to it needs a pull request; "that cost lands rarely and on the right person, the one who chose the dependency." And a read-only library's facts go unverified on the consumer's machine, deliberately: "the stamps a consumer reads are the publisher's."

Not covered by the decision:

- **Holding both same-named libraries on one machine.** A consumer whose own library collides with a dependency's still cannot install the dependency. The decision makes the collision visible and does not resolve it; aliasing is the way, "if a real case needs it."
- **Filing upstream issues automatically.** A skill names where to file; a person files.
- **A dependency's version.** `links_to` names a repository, not a release, so a slug the publisher's dependency gained after the consumer's copy is still a dead link, reported with the origin.

Source: [[adr-0020-published-libraries-and-consumers]] · [[engine-changelog-v0.4.0]]
Verified: 2026-11-11 · by: agent · method: doc-review
