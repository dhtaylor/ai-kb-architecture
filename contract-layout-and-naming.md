---
name: contract-layout-and-naming
description: The contract rules for where a fact lives and what a library looks like — scope tiers and the product-versus-general swap test, the comparative product library, the three homes, how the engine location resolves, domain layout, the INDEX.md router and the fields a root INDEX declares, granularity, slugs and cross-library links, the frontmatter schema, naming — with ADR-0003 (three homes), ADR-0004 (scope attribute, check-scope) and ADR-0010 (cross-library links checked at commit).
memory_type: semantic
domain: knowledge-architecture
scope: general
verified: 2026-11-21
metadata:
  type: fact
  node_type: memory
  created: 2026-11-20
tags: [knowledge-architecture, contract]
keywords: [contract, CONVENTIONS, scope, repo:, product:, org:, general, swap the vendor, comparative product library, vendors, mirror rule, three homes, literal path, KB_ENGINE_ROOT, kb-bootstrap, INDEX.md, router, hooked pointer, descent rule, freshness_horizon, verifier_budget, sweep_interval, last_swept, links_to, Not covered here, state leaf, needs-attention, gestalt, one fact one home, slug, path-addressed, cross-library link, soft dependency, dead link, check-xlinks, ADR-0003, ADR-0004, ADR-0010, check-scope, frontmatter schema, memory_type, naming, kebab-case, precedence]
---
# Contract: layout, scope, routers, slugs and naming

Section numbers written as § are the contract's own, in `ai-kb:CONVENTIONS.md`; the contract stays the authority and this file restates its rules with provenance, copying their values and qualifiers. Contract versions and `links_to:` are in [[releases-and-contract-versioning]]; provenance and currency are in [[contract-provenance-and-currency]]; retrieval and the list of checks are in [[contract-retrieval-and-verification]].

## Precedence: this contract wins over a skill, and the design plan wins over this contract

**Precedence:** the combination plan is the design authority; this file is the operative contract derived
from it; skills conform to this file. When a skill and this file disagree, **this file wins**. When this
file and the plan disagree, the plan wins and this file is amended — not worked around.

Source: [[conventions-contract-rules]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'the plan wins and this file is amended' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## Every fact carries a scope: the five values and where each lives

*Rationale: [[adr-0003-scoping-topology]] for the three homes and why the project nests inside the general root; [[adr-0004-scope-attribute]] for the values and what enforces them.*

Every fact carries a `scope:`. The classification test, asked when the fact is written:

> *Is this true only of this repo/deployment, or true wherever this product/tool/concept appears?*

| `scope:` value | Meaning | Home |
|---|---|---|
| `repo:<project>` | True only of this deployment — topology, deploy quirks, this system's bugs | `<project>/knowledge/` |
| `product:<vendor>` | True wherever that vendor's product appears | general root |
| `org:<company>` | True across the organization — standards, architecture rules | general root |
| `general` | Domain fundamentals, engine/platform behavior | general root |
| *(personal)* | An individual's working style and private lessons | `~/.claude/` — **not** team-shared, never in either KB |

Source: [[conventions-contract-rules]] · [[adr-0003-scoping-topology]] · [[adr-0004-scope-attribute]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'Is this true only of this repo/deployment, or true wherever this product/tool/concept appears?' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## Product scope or general scope: the swap test

**`product:<vendor>` and `general` are not separated by the words above.** For a single-vendor
engine, "that vendor's product behavior" and "domain fundamentals" describe the same sentence. The
test that does separate them:

> *Would this fact stop being true if you swapped the vendor or product?*

**Yes → `product:<vendor>`** — how a specific database implements snapshot isolation.
**No → `general`** — what serializable isolation means, or what the standard requires.

They are not interchangeable: §9 applies different promotion guards, and a vendor fact filed as
`general` is a claim about the whole world inferred from one product. Never classify from the
vocabulary a requester happened to use.

Source: [[conventions-contract-rules]] · [[adr-0004-scope-attribute]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'Would this fact stop being true if you swapped the vendor or product?' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## A comparative product library: product:category scope and the vendors list

**A comparative product library compares several vendors' products: `product:<category>`, plus
`vendors:`.** Some knowledge is about choosing between alternatives, such as a message-broker
capability matrix with notes on each broker. `general` would make each vendor fact a claim about the
whole world, and one library per vendor leaves the comparison with no home. So a library may take
`scope: product:<category>` and list, in its root `INDEX.md`, the vendors it compares:

```yaml
scope: product:message-brokers
vendors: [kafka, rabbitmq, azure-service-bus, amazon-sqs]
```

It is still one library and one tier, and every file carries the library's scope. The rules:

- **A category is the alternatives a reader would compare for one job.** Message brokers are; "cloud"
  is not. A category broad enough to need no comparison is `general` under another name. A
  comparative library exists to hold a comparison, not to save on repositories: never merge
  single-vendor libraries into one for convenience.
- **Every vendor-specific fact names its vendor, in its heading or its sentence.** In a
  single-vendor library the scope says whose product a fact describes. Here only the section can, and
  a section that forgets reads as true of every product in the category: the `general` error, made
  inside the library. The swap test applies per section.
- **A fact true of every product in the category is `general`.** What at-least-once delivery means
  goes in a `general` library and is linked, never restated.
- **Where a single-vendor library exists, link to its facts; never restate them.** A Kafka fact
  written in both a `product:kafka` library and a comparative one is two copies that drift (§4).
- **Stamp per vendor.** Vendors release on different schedules, so a matrix under one stamp is
  partly stale before the stamp says so. Keep each vendor's facts in its own stamped section. A matrix
  that summarises them is only as fresh as its oldest row, so its stamp is never newer than the
  oldest section it draws on.

`check-scope` checks what it can: `vendors:` names at least two vendors, each a slug, none twice;
the scope is `product:`; and the category is not one of the vendors. Whether a slug really names a
vendor, and whether a category is narrow enough, are review's job.

Source: [[conventions-contract-rules]] · [[adr-0004-scope-attribute]] (the 2026-10-16 amendment)
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'A category is the alternatives a reader would compare for one job.' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## A library is one domain and one tier, and a project tree obeys the mirror rule

**A library is one domain, and a domain is one tier.** Every file in a library carries the same
`scope:` as the library itself. A `repo:` fact filed in a general-tier library is a claim about every
deployment inferred from one; a `product:` fact filed as `general` is a claim about every product
inferred from one. Both read as ordinary facts and neither announces itself, which is why
`check-scope` enforces the agreement rather than trusting it.

**A project tree obeys the mirror rule.** A library is its own repository and a general-tier home, so
`repo:` is wrong there. A project tree sits inside the project's repository and is the repo-tier home,
so **anything but `repo:` is wrong there** — a fact true beyond one deployment, filed into one project,
is one of N copies that will drift. `check-scope` tells the two apart by structure (does the tree have
its own `.git`?), not by name or path.

Source: [[conventions-contract-rules]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'A project tree obeys the mirror rule.' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## The three homes, and why behaviour is not one of them

The general home, a domain library installed by being registered on a machine, is described in [[design-thesis]]. The other two homes, and the rule about behaviour, as the contract states them:

- **Repo** — `knowledge/` inside the project repo, travelling with the clone.
- **Personal** — user-level config.

**Behaviour is not one of the homes.** The engine (this contract, the tooling, the skills) holds no
domains at all. Mixing them would be the very thing §0 of the design forbids — knowledge and
behaviour in one place — and the engine is where the separation has to be observed most visibly,
because everything else follows its example.

Source: [[conventions-contract-rules]] · [[adr-0003-scoping-topology]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'Behaviour is not one of the homes.' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## How the engine location is resolved: no literal paths, one command, one hook

**Never write a literal path.** A hardcoded absolute path is correct on exactly one machine and fails
silently everywhere else. But agent bodies do **not** write `$KB_ENGINE_ROOT/...` either — see below.

**Resolution mechanism — settled by the Phase 0 acceptance test (2026-09-19), see
[[adr-0001-kb-root-resolution]], and extended to every
consumer by [[adr-0008-machine-bootstrap]]:**

1. **One command writes every location: `kb-bootstrap`.** The engine is wherever that script lives, so
   nothing is typed and nothing is typed twice. Run once per machine, and again if the engine moves.
   Hand-editing any of the locations below is how two of them come to disagree.
2. `KB_ENGINE_ROOT` is set in **user-level settings** (`~/.claude/settings.json` `env`), not a project
   file — a project file is a trust-root hazard, since a repo could repoint every agent's canonical
   knowledge.
3. A **SessionStart hook, which lives in the engine** (`scripts/kb-session-start`, so it is versioned
   with the contract it announces), injects the **absolute path** into session context. Agents use
   *that* path. This is required, not cosmetic: **the Read tool does not expand environment variables.**
   Handed `$KB_ENGINE_ROOT/...` it fails with "File does not exist" and helpfully reports the working
   directory — inviting a relative-path retry that succeeds by coincidence in one workspace and breaks
   in every other. The hook removes the variable from the path-handling path entirely.
4. **Git hooks resolve the engine through `git config --global kb.engineRoot`**, after the environment.
   Git reads its global config in every context — terminal, IDE, GUI client, cron — and no shell startup
   file does: a stock `~/.bashrc` returns before its exports for any non-interactive shell, so a hook
   fired by anything but a human's own terminal would silently skip its checks.
5. **Absence is graceful and loud.** Unset or pointing nowhere: the session hook warns at session start
   and instructs agents to report the engine as unconfigured — never to guess a path, never to fall back
   to a relative one. A git hook says which command fixes it.

So: no file anywhere contains a literal KB path, no agent ever handles the variable itself, and every
place that needs the location is written by one command from one source.

A project fact that depends on a general fact **links to it** (§5), never restates it. The
single-canonical-home rule (§4) spans the KB boundary.

Source: [[conventions-contract-rules]] · `ai-kb:documents/decisions/0001-kb-root-resolution.md` · [[adr-0008-machine-bootstrap]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'the Read tool does not expand environment variables.' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## ADR-0003, the three homes: status, drivers, options, the nested test project and consequences

### Status and the decision

- **Status:** Accepted, amended 2026-09-19 by [[adr-0006-behaviour-distribution]] and [[adr-0007-engine-content-separation]]

> **Amended 2026-09-19 by [[adr-0006-behaviour-distribution]].** This ADR placed the general
> tier at `ws_v1/knowledge/` as a directory inside the workspace repository. It is now its own git
> repository at that path, carrying the decision records, guardrails and behaviour with it. The
> locations below are unchanged — `$KB_GENERAL_ROOT/knowledge/` still resolves identically — but the
> repository boundary has moved, and the reasoning about what a project can reach is extended there.

**Chosen: option 3, three homes.**

| Tier | Home | Version control |
|---|---|---|
| General (`general`, `product:*`, `org:*`) | `$KB_GENERAL_ROOT/knowledge/` | the `ws_v1` repository |
| Repo (`repo:<project>`) | `<project>/knowledge/` | the project's own repository |
| Personal | `~/.claude/` | none — deliberately not team-shared |

The ADR also names the first project and the general tier by absolute path on the maintainer's machine. Those paths are not recorded here.

Source: [[adr-0003-scoping-topology]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'Knowledge falls into tiers with different owners' "$KB_ENGINE_ROOT/documents/decisions/0003-scoping-topology.md"

### Why three homes: the problem, the drivers and the options considered

Knowledge falls into tiers with different owners and different rates of change, and getting
the middle tier wrong is how a shared knowledge base fails. Over-share, and every repository
couples to a churning blob. Under-share, and the same fact is copied into N repositories and
drifts — the exact failure the governed KB exists to prevent, merely relocated from files to
repositories.

This must be settled before the pilot, because every retrieval path the pilot writes is
hardcoded against it.

- **One canonical home per fact.** A fact stated twice will eventually disagree with itself.
- **A project must stay usable standalone.** A developer without the general tier gets a
  degraded answer, never a crash.
- **No literal paths.** Settled in [[adr-0001-kb-root-resolution]].
- **The topology must be testable**, not merely asserted, before it is built on.

1. **Single repository, `scope:` field only.** Defers the second root; no cross-root resolver
   needed. But the general tier never actually separates, and extracting it later means
   re-reading everything.
2. **Two roots** — general plus repo.
3. **Three homes** — general, repo, and personal.

Source: [[adr-0003-scoping-topology]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'Over-share, and every repository' "$KB_ENGINE_ROOT/documents/decisions/0003-scoping-topology.md"

### The nested test project: why a project sits inside the general repository

The architecture says the general repository is cloned to a location *outside* any single
project. Here the project sits **inside** the general repository's directory tree, at
`code/test_project/`.

This is safe only because of a specific property, and it would be unsafe without it:
`test_project` is **its own git repository**, and the parent `.gitignore` excludes `code/`.
The two tiers are therefore genuinely separate *version-control* roots even though they are
nested on disk. Neither can commit the other's files; neither appears in the other's history.

The reason to accept the nesting rather than "fix" it: it makes the cross-root machinery
**testable on one machine**. A project that must resolve `[[general:slug]]` through a
configured root — rather than through a convenient relative path — exercises the resolver, the
slug manifest and graceful absence for real. Had the two tiers shared one repository, relative
paths would quietly work and the resolver would never be exercised until it failed on someone
else's machine.

The risk this accepts: nesting invites someone to reach across with a relative path, since the
files are right there. `check-embedded-facts` flags absolute paths; a relative traversal like
`../../knowledge/` is not currently caught. **Added to the check-scope backlog** ([[adr-0004-scope-attribute]]).

Source: [[adr-0003-scoping-topology]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'it makes the cross-root machinery' "$KB_ENGINE_ROOT/documents/decisions/0003-scoping-topology.md"

### Versioning stance: latest-wins for humans, pin for automation

**Latest-wins for humans; pin for automation.** Code dependencies are pinned for reproducible
behavior; facts are the opposite — a correction should propagate immediately, not wait behind
a version bump in ten repositories. Where the general tier feeds something *unattended* (a CI
eval, a scheduled agent), non-determinism bites, so those pin to a tagged ref.

With one contributor and one machine this is currently moot: there is nothing to pull from and
no second consumer to surprise. It is recorded now because the moment a second machine exists,
the default is already decided.

Source: [[adr-0003-scoping-topology]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'facts are the opposite' "$KB_ENGINE_ROOT/documents/decisions/0003-scoping-topology.md"

### ADR-0003 consequences and open items, as the ADR first stated them

**Good.** Each fact has exactly one home. The general tier is owned by no single project. A
project KB travels with its clone and is reviewed alongside the code it describes.

**Bad.** A cross-root `[[general:slug]]` resolver **does not exist**, and until it does every
cross-repo link is unvalidated — dead links would accrue silently. This blocks Phase 3, per the
plan. `check-kb` today resolves links within a single root only and says so.

**Neutral.** Personal knowledge is excluded from both repositories by construction rather than
by convention, which means there is no mechanism to accidentally share it and no mechanism to
back it up either.

- The cross-root resolver and the slug→path manifest (Phase 3 blocker).
- A relative-traversal check, so nesting cannot be abused (see above).
- The bootstrap command that provisions a new machine ([[adr-0001-kb-root-resolution]]).
- Latest-wins is untested: there is no second consumer yet to propagate a correction to.

These were stated on 2026-09-19. Later, section 13 of the contract lists `check-xlinks` as built, and section 5 says no slug manifest is generated; both are in [[contract-layout-and-naming]] and [[contract-retrieval-and-verification]].

Source: [[adr-0003-scoping-topology]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'The general tier is owned by no single project.' "$KB_ENGINE_ROOT/documents/decisions/0003-scoping-topology.md"

## ADR-0004, the scope attribute: required frontmatter, the tie-break, what check-scope enforces

### Scope is required frontmatter on every semantic and reference file

| Value | Means | Home |
|---|---|---|
| `repo:<project>` | True only of this deployment | project repository |
| `product:<vendor>` | True wherever that vendor's product appears | general |
| `org:<company>` | True across the organization | general |
| `general` | Domain fundamentals, engine/platform behavior | general |

Personal knowledge carries no `scope:` because it is never in either repository.

**Amended 2026-09-19.** The four values above are not separable by their descriptions alone. The
first live use of `create-domain` misclassified PostgreSQL MVCC as `general` when it is
`product:postgresql`, because "engine behavior" and "that vendor's product behavior" are the same
sentence for a single-vendor engine. The tie-break, now in the contract §1 and in the skill:

> *Would this fact stop being true if you swapped the vendor or product?* Yes → `product:<vendor>`.
> No → `general`.

This matters beyond tidiness: the two tiers carry different promotion guards, and a vendor fact
filed as `general` asserts about every product what was observed of one.

This goes into the capture skills so it is answered every time. A fact whose answer is "both"
is two facts, and is split.

Source: [[adr-0004-scope-attribute]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'Would this fact stop being true if you swapped the vendor or product?' "$KB_ENGINE_ROOT/documents/decisions/0004-scope-attribute.md"

### The rules check-scope was specified to enforce, with built or to-build status

Specified now; **not yet built**. `check-kb` implements the third rule only.

| Rule | Why | Status |
|---|---|---|
| `scope:` present and a valid value | The attribute is useless if optional | to build |
| Scope agrees with the root the file sits in — a general-root file may not be `repo:*` | Catches a fact written into the wrong tier | to build |
| Slug uniqueness **within** a root; `INDEX.md` and `CONVENTIONS.md` exempt as path-addressed | Bare `[[slug]]` must resolve unambiguously | **built** (`check-kb`) |
| Slug uniqueness **across** roots | `[[general:slug]]` must resolve unambiguously | to build (Phase 3) |
| No `[[slug]]` targets a path-addressed file | Routers are reached by path; a link to one is a mistake | **built** (`check-kb`) |
| No relative traversal out of a root (`../../knowledge/`) | The nested layout of ADR-0003 makes this reachable and it would bypass the configured root | to build |
| Promotion to the general tier carries a CODEOWNER scope audit | The general tier is what every project depends on | to build (dormant, one owner) |

**Status 2026-09-19.** `check-scope` is built and covers rows 1, 2 and 6 — well-formed values, agreement
between a file and its library, and no relative link climbing out of a library. Row 4 (**cross-root slug
uniqueness**) is **obsolete, not outstanding**: every cross-library reference now carries a mandatory
`[[library:slug]]` prefix, so nothing can collide and there is nothing left to enforce. Row 7 (the
CODEOWNER audit) stays dormant while there is one owner.

Source: [[adr-0004-scope-attribute]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'Row 4 (**cross-root slug' "$KB_ENGINE_ROOT/documents/decisions/0004-scope-attribute.md"

### ADR-0004 consequences and open items

**Good.** Scope backfill — which the architecture warns is real work on a grown KB — is trivial
here: six files, all `general`. Starting with the attribute enforced means the silent mixing
never happens rather than being cleaned up later. This is the cheapest this decision will ever be.

**Bad.** Most of `check-scope` does not exist, so most of the table above is currently honored by
discipline alone. The two rules that *are* built are the ones `check-kb` enforces on every commit;
the rest are a backlog, and the table says which is which rather than implying coverage.

**Neutral.** `scope:` is redundant with the file's location today, since one root means one
possible value. It earns its keep when the second root has content — and it must be present
before then, because retrofitting it is the thing this decision exists to avoid.

- Build `check-scope` and wire it into the pre-commit hook and CI beside `check-kb`.
- Cross-root uniqueness needs the second root populated (Phase 3).
- The CODEOWNER scope audit is dormant until there is more than one owner ([[adr-0002-governance-without-enforcement]]).

Source: [[adr-0004-scope-attribute]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'The two rules that *are* built are the ones' "$KB_ENGINE_ROOT/documents/decisions/0004-scope-attribute.md"

### ADR-0004 amendment: the comparative product library

`product:<vendor>` had no answer for knowledge that compares several vendors' products
(#37), such as a message-broker capability matrix.
`general` would make each vendor fact a claim about the whole world, one library per vendor leaves
the comparison with no home, and a category written as `product:message-brokers` passed
`check-scope` while naming no vendor at all. **A comparative product library is now sanctioned:
`scope: product:<category>`, with a `vendors:` list in its root `INDEX.md`.** It remains one
library and one tier, so the agreement rule, the promotion guards and `check-blast` are unchanged.
CONVENTIONS §1 adds the rules that tooling cannot check: what a category is, a vendor named in every
vendor-specific section, links rather than restatement where a single-vendor library exists, and
stamps per vendor. `check-scope` validates the list's shape. A per-file vendor scope inside one
library was considered and rejected: it breaks one-library-one-tier, and the matrix, which spans
every vendor, would still have no single scope.

Source: [[adr-0004-scope-attribute]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'would still have no single scope.' "$KB_ENGINE_ROOT/documents/decisions/0004-scope-attribute.md"

## Domain layout: the library root is the domain, the library home, and flat until about 15 files

A library is a repository whose root is the domain, with no tier folder above it, installed by being registered; that part is in [[design-thesis]], and a local-only library is described there too. What else the contract says about layout, from the end of its first bullet onward (the first line below begins mid-sentence, at the library's declared name):

  `domain:`. The registry is per machine and outside every repository, so registering writes nothing
  into a library ([[adr-0014-library-registry]]). `kb-library add <git-url>`
  clones into the **library home** (`$KB_LIBRARY_HOME`, else the home `kb-bootstrap` recorded, else
  `<engine>/kb/`), which is only where new clones go; nothing is found by being there.
  `kb-library export` and `import` carry a machine's set of libraries, by name and origin, to another.

- Content lives in **single-topic files** sized as described in §4.
- When a domain's topic files exceed **~15**, group the overflow into a sub-folder with its own
  sub-index. Below that threshold, stay **flat** — every sub-folder adds a routing hop.

Source: [[conventions-contract-rules]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'group the overflow into a sub-folder with its own' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## The router: an INDEX.md lists only its direct children, one hooked line each

Every folder — the domain root and each sub-folder — carries an `INDEX.md` that lists **only its
direct children**, one line each, and nothing else. A reader descends the tree one hop at a time,

Each record is a hooked pointer:

- a **leaf** file: `- [key](child.md) — one-line hook (what it answers)`;
- a **sub-folder**: `- [key](subfolder/INDEX.md) — what lives under here`.

`key` is the child's slug (§5). The hook must **disambiguate siblings** — two children must never be
distinguishable only by a shared prefix.

**Descent rule:** load a folder's `INDEX.md`, pick the one child whose hook fits the question; if

**Descent rule:** load a folder's `INDEX.md`, pick the one child whose hook fits the question; if
that child is itself a sub-index, load it and repeat.

**Bound the index.** Above the ~15 threshold, split into sub-indexes so an agent loads only the relevant
one. The fixed agent preamble plus the INDEX belong in the cacheable prefix so repeat queries don't re-pay

Source: [[conventions-contract-rules]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'distinguishable only by a shared prefix.' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## The fields a root INDEX.md declares in its frontmatter

The router holds routing signal and **domain configuration** only — no stored facts. A domain root
`INDEX.md` additionally declares, in its frontmatter:

```yaml
freshness_horizon: <N days>   # the domain's TTL floor; volatile deployment facts decay in days,
                              # fundamentals in years. One interval for the whole library is wrong.
verifier_budget: <N>          # max facts re-checked per verification cycle, highest score first.
                              # Without a cap the cadence grows unbounded as domains multiply.
sweep_interval: <N>d          # how long a kb-audit sweep stays good for. 30d for a library, 14d for
                              # a project tree, whose facts churn faster. Explicit rather than derived
                              # from the horizon: a 90d horizon would imply a 9d nag.
vendors: [<vendor>, ...]      # a comparative product library only (§1): the vendors its
                              # product:<category> compares. At least two; check-scope checks it.
unasserted_ttl: <N>d          # optional, contract 2.7: the short lease of an unasserted stamp, one
                              # with no passing Recheck: behind it, whoever wrote it. Default a
                              # tenth of the horizon, at least 1d; check-kb fails one not shorter
                              # than the horizon (§7). Below 2.7 the field is agent_ttl: (§14).
kb_contract: "<MAJOR.MINOR>"  # optional: the contract version (§14) this library was built to.
                              # Absent is read as 2.0. Quoted, always: a YAML reader turns a bare
                              # 2.10 into the number 2.1. check-kb fails a bare or malformed one.
last_swept: YYYY-MM-DD        # written by kb-audit at the end of a sweep, `never` until the first.
                              # It lives here, not in needs-attention.md, because a CLEAN sweep
                              # writes no queue file and would erase the evidence that it ran.
links_to:                     # every library a [[library:slug]] link names, and the origin (git
  <library>: <origin>         # remote, or the absolute path of a local-only library) it was linked
                              # against (§5, contract 2.6). Required by a tree that links across
                              # libraries; check-kb fails one missing.
```

Superseded: until engine v0.5.0 the block had `agent_ttl: <N>d` here ("optional: how long an agent stamp stays fresh"); contract 2.7 renames it (see [[releases-and-contract-versioning]]).

Source: [[conventions-contract-rules]] · [[adr-0022-evidence-not-identity-sets-the-lease]]
Verified: 2026-11-24 · by: agent · method: doc-review
Recheck: grep -qF 'unasserted_ttl: <N>d          # optional, contract 2.7: the short lease of an unasserted stamp, one' "$KB_ENGINE_ROOT/CONVENTIONS.md" && ! grep -qF 'agent_ttl: <N>d               # optional' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## The Not covered here line: its form and what it may name

A library's root `INDEX.md` may also carry one body line saying what the library does **not** hold,
so a reader choosing between neighbouring libraries can rule one out (contract 2.3):

```
Not covered here: <neighbouring topics>, <more> — held by [[other-lib:slug]] or <library name>
```

It is routing signal, not a stored fact: name topics, never state facts about them. Name the library
that holds a topic where one is registered; a topic no library holds is named without one. It is
optional, and `check-kb` checks only that the line has content after the colon. The `[[library:slug]]`
form resolves like any cross-root link (§5, `check-xlinks`).

Which contract version added the line, and how a retrieval agent uses it, is in [[releases-and-contract-versioning]] and [[retrieval-agent-behavior]].

Source: [[conventions-contract-rules]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'Not covered here: <neighbouring topics>, <more>' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## State leaves, the needs-attention queue, and what a router deliberately does not route

Cross-cutting material that isn't a routing decision lives in its own child doc, listed like any other
record: unresolved items in `<domain>-open-questions.md`, the contradiction register in
`<domain>-contradictions.md` (§8). Create these state leaves **only when they hold content** — don't
route to an empty file.

The knowledge base carries one root-level state leaf, `needs-attention.md` — the queue a sweep builds.
It is **a file like any other**: full frontmatter (§11) carrying the library's own `domain:` and
`scope:`, routed from the library's own `INDEX.md`,
because a queue that is itself an orphan is the first thing the next sweep will report. It belongs to
the library it describes, not to the engine. Like every state leaf it
exists only when it has content.

Two things inside a library are deliberately **not routed**: the golden set (§10) and archived
evidence under `documents/` (§6). They are apparatus and provenance, not knowledge — and routing the
golden set would let a retrieval agent read the answers it is being tested on. A sweep must not
report either as an orphan.

Source: [[conventions-contract-rules]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'They are apparatus and provenance, not knowledge' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## Granularity: a file is a gestalt and one fact has one home

- **One fact, one home.** A fact is stated in full in exactly one file. Everywhere else it is
  referenced with a one-line `[[cross-link]]` at the point of use — never restated.
- A file is right-sized when it can be loaded on its own to answer a real question, yet still holds a
  **coherent whole** rather than one fact per file. Don't over-fragment a coherent model to honour the
  rule: an entity catalog, its relationship diagram, its business rules, and its code tables usually
  belong **together** because they are loaded together. Split out only what is genuinely queried alone
  (e.g. process flows, gotchas).
- **Consequence:** a file holds several facts of differing volatility and differing provenance.
  Therefore provenance (§6) and currency (§7) attach to the **section**, not the file.

Source: [[conventions-contract-rules]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'A fact is stated in full in exactly one file.' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## Slugs: the filename without .md, unique within a library, and the path-addressed exemption

- **`slug` = the filename without `.md`.** Not a frontmatter field. A `[[slug]]` names a file, never
  a folder: point it at the file that answers, not at a router. A folder name is still reserved: no
  file may share it, so a slug never reads as a folder (`check-kb`).
- **Bare `[[slug]]`** resolves within the KB root it appears in. Filenames are therefore **unique within
  a root** — which is why per-domain state leaves take a domain-prefixed *filename*
  (`<domain>-contradictions.md`), not a prefixed frontmatter name.
- **Path-addressed files are exempt.** Two kinds of file are reached by **relative path**, never by
  `[[slug]]`: the `INDEX.md` routers (every folder carries one, §3, so the basename necessarily repeats)
  and this contract at the KB root. Because nothing resolves *to* them, nothing can resolve ambiguously,
  so they keep their conventional discoverable filenames and carry a descriptive `name:` instead of one
  matching the filename (`<domain>-index`, `sources-index`, `knowledge-conventions`). `check-scope`
  exempts them from the within-root uniqueness check and **fails any `[[slug]]` that targets one** — cite
  the contract by path, or cite the specific fact that restates its rule.

Source: [[conventions-contract-rules]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'file may share it, so a slug never reads as a folder' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## Cross-library links: the library prefix, resolution one level up, soft versus dead

- **Cross-library links name the library:** `[[<library>:<slug>]]`, e.g.
  `[[claude-code-runtime:behaviour-loading]]`. The prefix names the **library**, not a tier — the older
  `[[general:slug]]` form came from a two-root model where "general" identified a single tree; with N
  libraries installed it identifies nothing.
- **Slugs need be unique only *within* a library.** Cross-library uniqueness is **not** required, and
  the rule that once demanded it is obsolete: an explicit prefix is mandatory for every cross-library
  reference, so `[[a:foo]]` and `[[b:foo]]` cannot be confused. A bare `[[slug]]` never leaves the
  library it is written in, so there is nothing left to collide.

- **No manifest.** A generated slug→path manifest was specified so a consumer could resolve against a
  *remote* root without scanning it. Every installed library is local and registered, so resolution
  reads the registry and scans each library, and a manifest would be committed state that can go
  stale for a problem that no longer exists. Add one when a remote library exists to justify it.

- **Resolution runs one level up.** A library cannot see its siblings, so `check-kb` does not resolve
  prefixed links, and `check-xlinks --installed` resolves them across every registered library. That
  split is not an implementation detail: it is why a library remains checkable on its own.

- **Graceful absence is a distinction, not a leniency.** A link into a library that is **not installed**
  is a *soft dependency*: it warns, and agents continue. A link into a library that **is** installed but
  lacks the slug is **dead** and fails. Collapsing the two would punish portability — a machine need not
  hold every library — while treating both as warnings would let real rot accumulate unseen.

The rule that a link names where its library comes from, `links_to:` and dependency mismatches, and a local-only library's origin as its path, are in [[releases-and-contract-versioning]].

Source: [[conventions-contract-rules]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'is a *soft dependency*: it warns, and agents continue.' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## ADR-0010: cross-library links are resolved at every commit that can break one, not in CI

### The gap ADR-0010 closed

`check-xlinks` exists and is regression-tested in `tests/run-checks`, but nothing ran it against
real content at the point a `[[library:slug]]` link can actually break:

- A library's own CI (`kb/*/.github/workflows/guardrails.yml`) deliberately skips it — a lone
  library cannot see its siblings, so the job says so and moves on.
- The engine's pre-commit hook (`.githooks/pre-commit`) runs `check-xlinks kb`, but only fires on a
  commit made *in the engine*. A slug rename or removal happens in a library's own repository.
- Neither library's pre-commit hook called `check-xlinks` at all.

So a slug renamed or removed in library A, cited by library B via `[[A:slug]]`, committed cleanly in
A. The dead link would sit unnoticed until someone next committed to the engine and happened to run
its hook, or ran `kb-audit` by hand. Phase 3 of the combination plan names this exact gap as its
blocker: "the cross-root `check-links` resolver existing and running in CI."

Source: [[adr-0010-cross-library-links-checked-at-commit]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'So a slug renamed or removed in library A' "$KB_ENGINE_ROOT/documents/decisions/0010-cross-library-links-checked-at-commit.md"

### The three options ADR-0010 weighed

1. **A credentialed integration CI job.** A workflow with access to both private library repos,
   triggered on push to either, running `check-xlinks` over both checkouts. This is the design the
   plan's Phase 3 sentence assumed. Rejected: the libraries are private, so the job needs a read
   token for at least one of them — some credential, stored somewhere, rotated by someone. That
   directly contradicts the reason `guardrails.yml`'s own header comment gives for fetching the
   engine anonymously: "this needs NO credential: no secret to add, none to rotate, and nothing to
   leak." A job that resolves cross-library links needs exactly the credential that design exists to
   avoid. It also cannot live in the public engine repository — a run there would log which slugs and
   paths exist in a private library, which is the leak the whole split (public behavior, private
   knowledge) is built to prevent.
2. **Leave it to `kb-audit` alone.** `kb-audit` already sweeps for dead links as part of its decay
   detection. Rejected as the *only* gate: it is a sweep run on a cadence or by hand, not a commit
   gate, so a broken citation can sit for a full sweep interval looking clean.
3. **Run `check-xlinks` at every commit that can break a link, engine and library alike, and keep
   `kb-audit` as the periodic backstop.** Chosen.

Source: [[adr-0010-cross-library-links-checked-at-commit]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'A job that resolves cross-library links needs exactly the credential that design exists to' "$KB_ENGINE_ROOT/documents/decisions/0010-cross-library-links-checked-at-commit.md"

### The decision: check at every commit that can break a link, and what the library hook does when it cannot see siblings

**Chosen: option 3.** The Phase 3 gate is now: cross-library links are resolved at every commit that
can break one — the engine's existing hook, plus each library's own pre-commit hook, when that
library's siblings are visible to it — with `kb-audit` catching anything that slips past both.

The library hook can only resolve truthfully when it can see the other libraries, which means it
lives at `<engine>/kb/<itself>` — the designed layout, or a clone that ran `kb-bootstrap` and points
`kb.engineRoot` at a real `kb/` directory holding siblings. The hook checks this with a realpath
comparison (`pwd -P` against `<engine>/kb/<this library's directory name>`) rather than assuming: a
library found via `kb.engineRoot` set by hand could be cloned anywhere, engine included, with no
sibling underneath it at all. When the comparison fails, the hook prints one line saying
cross-library links were not checked and exits clean — absence warns, it never fails, the same rule
`check-xlinks` itself applies to a library that is not installed. This is not a fallback bolted onto
CI; portability is why the split (§5 of the contract: a library "cannot see its siblings") exists in
the first place, and the hook's silence-vs-skip behavior is a direct expression of it.

Source: [[adr-0010-cross-library-links-checked-at-commit]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'absence warns, it never fails' "$KB_ENGINE_ROOT/documents/decisions/0010-cross-library-links-checked-at-commit.md"

### What ADR-0010 accepts as incomplete, and the trigger to revisit

**This is accepted as incomplete, on the record:**

- **Hooks are bypassable.** `--no-verify` skips them outright, and a clone that never ran
  `kb-bootstrap` has no hook installed at all (`core.hooksPath` is never set). Both were already true
  of every other pre-commit check in this system; this decision does not change that boundary, it
  extends the same boundary to cross-library links.
- **A library cloned outside an engine's `kb/`** — siblings not installed alongside it — gets no
  cross-link check from its own hook, by design (see above). Its citations are only checked when
  someone later commits from inside an engine that has it installed, or when `kb-audit` runs.

Both residuals are accepted, not overlooked. The mitigation is `kb-audit`, which sees the whole `kb/`
tree regardless of which repository's commit triggered it. There is no server-side backstop for
cross-library links: the server-side gates run per repository and cannot see siblings, which is the
gap option 1 would have closed.

**Trigger to revisit:** a third library (three-way link resolution starts to matter more, and the
`ENGINE/kb` realpath check gets exercised against more topologies), or a second contributor
committing to these libraries (bypass and never-bootstrapped clones stop being a single person's own
risk to accept).

Source: [[adr-0010-cross-library-links-checked-at-commit]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'There is no server-side backstop for' "$KB_ENGINE_ROOT/documents/decisions/0010-cross-library-links-checked-at-commit.md"

### ADR-0010 consequences

- The plan's Phase 3 "Blocked on" sentence is resolved: the blocker was CI-shaped, the fix is
  commit-shaped, and Phase 3 can proceed.
- `tests/run-checks` gained cases that exercise the *template* hook in
  `kb-create-domain/SKILL.md` directly, not a copy of it, so a future edit to the template that
  silently drops or breaks this block is caught the same way every other guardrail regression is.
- No new credential exists anywhere in this system. That was the constraint that ruled out option 1,
  and it still holds.

Source: [[adr-0010-cross-library-links-checked-at-commit]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'No new credential exists anywhere in this system.' "$KB_ENGINE_ROOT/documents/decisions/0010-cross-library-links-checked-at-commit.md"

## The frontmatter schema every fact file carries

Every semantic/reference file carries:

```yaml
---
name: <kebab-case — matches the filename; path-addressed files excepted, see §5>
description: <one line — what this file answers, disambiguating from siblings>
memory_type: semantic        # or: reference
domain: <domain>
scope: <repo:<project> | product:<vendor> | org:<company> | general>
verified: YYYY-MM-DD         # the floor: oldest section stamp in this file (§7)
metadata:
  type: <fact | reference | ...>
  node_type: memory
  created: YYYY-MM-DD
tags: [<domain>, <topic>, ...]
keywords: [<retrieval terms an agent might grep for>]
---
```

Section-level `status: CONFLICTED` and `status: UNCORROBORATED` (§8), and `provenance: BROKEN` (§6, written by a person after
triaging a dead source) are written **inline at the section**, not in frontmatter — they describe a claim, not a document.

`tags`/`keywords` exist so retrieval can route by grepping frontmatter — keep them from a shared
vocabulary within a domain, not ad-hoc per file.

Source: [[conventions-contract-rules]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'Every semantic/reference file carries:' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## Naming: kebab-case, self-describing, unique within a root

Files are kebab-case and **self-describing** (`data-model.md`, not `bnp-ontology.md`). The name should
tell a cold reader what's inside without a parenthetical. Since the filename *is* the slug (§5), it must
be unique within its root — per-domain state leaves take a domain prefix. The path-addressed files of §5
(`INDEX.md`, `CONVENTIONS.md`) keep their conventional names and are exempt.

Source: [[conventions-contract-rules]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'tell a cold reader what' "$KB_ENGINE_ROOT/CONVENTIONS.md"
