---
name: releases-and-contract-versioning
description: How the engine is versioned and released (semantic versioning, the two-day freeze, what a library release records) and why the library contract has its own MAJOR.MINOR version that a library declares, how rules apply by that version, what a library behind, ahead or on an unsupported major is told, what each contract version adds, and why pinning the engine is the library owner's business.
memory_type: semantic
domain: knowledge-architecture
scope: general
verified: 2026-10-28
metadata:
  type: fact
  node_type: memory
  created: 2026-10-28
tags: [knowledge-architecture, releases, contract, versioning]
keywords: [release, semantic versioning, minor bump, patch, code freeze, freeze, pinning, kb_contract, contract version, MAJOR.MINOR, declared version, behind, ahead, unsupported major, unsupported-contract, last two majors, conformance suite, Not covered here, Recheck (attended), sources, documents, migration notes, ADR-0015, ADR-0016]
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

A library is content, so a correction propagates when it lands, not at the next release. A library tag marks a known-good point, nothing more. A library's release notes name the contract version it conforms to, not an engine version.

Whether a library's CI pins the engine, to what, and when it moves is the library owner's decision. The engine neither requires nor tracks it, and an engine release moves no library's pin. The engine's CI template stays as an example, and pinning a release tag is still a sensible default for unattended use, which the example says is the owner's choice.

This replaced an earlier rule that every library's CI pin the engine to a release tag, with each release moving the pins. That rule assumed the engine's maintainer knows every library. The engine is meant for wide adoption, so the expected case is libraries owned by people the maintainer will never hear of, and the earlier decision's own review trigger, a library maintained by someone else, is the normal state.

Source: [[adr-0015-releases-and-pinning]] · [[adr-0016-versioned-library-contract]]
Verified: 2026-10-28 · by: agent · method: doc-review

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

An engine supports every minor of the current major and of the one before it, so the window spans years, not releases. When a new major is published the engine drops the major two behind it, and a library on the previous major is warned ahead of time that its major leaves support when the next is published.

A library on an unsupported or newer major is refused rather than served with a warning because its markers may mean something this engine does not read. Serving it "could serve a disputed fact as settled, which the retrieval contract exists to prevent."

Source: [[adr-0016-versioned-library-contract]] · [[script-check-kb]] · [[script-kb-libraries]] · [[conventions-contract-versions]]
Verified: 2026-10-28 · by: agent · method: doc-review
Recheck: f="$KB_ENGINE_ROOT/CONVENTIONS.md"; [ -f "$f" ] && grep -q '^### 2\.3$' "$f" && grep -q 'Absent, it is read as \*\*2\.0\*\*' "$f"

## What each contract version adds

The versions an engine knows are the `### X.Y` headings of the contract's section 14, and what each adds is its first paragraph. The text a library's owner is shown when the library is behind is read from there.

- **2.0** is the baseline: the rules a library met under the engine's first release. Every library that predates `kb_contract:` was built to it.
- **2.1** makes `sources/` hold only `.md` citation stubs. `check-kb` fails a raw artifact there, which belongs under `documents/` with a stub that cites it. Before the rule, `check-kb` read only `.md` files, so a raw file under `sources/` passed silently and its owner never learned it was in the wrong place. Migrating from 2.0 means moving any non-`.md` file under `sources/` to `documents/`.
- **2.2** adds `Recheck (attended):` for an assertion that needs a login, a credential or a live system. A scheduled `kb-verify --yes --unattended` skips it and counts it, and runs every other assertion with an empty `HOME` and only `PATH`, `KB_ENGINE_ROOT` and proxy settings. An unmarked assertion that needs a credential therefore fails rather than reaching one. Migrating from 2.1 means marking any assertion that needs a person and raising the declaration. The reason is in [[unattended-preparation]].
- **2.3** lets a root `INDEX.md` carry a `Not covered here:` line naming the neighboring topics the library does not hold and, where known, the library that does. `check-kb` fails a line with nothing after the colon, from this version, and warns below it. Nothing is required to migrate. How retrieval uses the line is in [[retrieval-agent-behavior]].

Source: [[conventions-contract-versions]] · [[script-check-kb]] · [[tests-run-checks]]
Verified: 2026-10-28 · by: agent · method: doc-review
Recheck: f="$KB_ENGINE_ROOT/CONVENTIONS.md"; [ -f "$f" ] && grep -q '^### 2\.1$' "$f" && grep -q '^### 2\.2$' "$f" && grep -q 'fails a line with nothing after the$' "$f"

## What the design costs

The decision states its own costs. Every new rejection carries a version check, and the engine keeps two majors' rules, which dropping the older major when a new one ships bounds. A library whose owner ignores two majors' warnings stops being served, which falls only on a library nobody has updated across two contract majors. The checks can no longer simply apply "the rules": each rule needs the version that introduced it, and the tests cover each rule both below and at its version. A library behind the contract is reminded at every session start until it moves, deliberately, and that may be noisy for an owner who has decided to stay.

The decision's delivery plan also named a conformance suite per contract version and a split of the contract so it holds only the library-facing spec. No source read records either as built; see [[knowledge-architecture-open-questions]].

Source: [[adr-0016-versioned-library-contract]]
Verified: 2026-10-28 · by: agent · method: doc-review
