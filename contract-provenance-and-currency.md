---
name: contract-provenance-and-currency
description: The contract rules for where a fact came from and whether it is still true — per-section Source lines, sources versus documents, citing an artifact, recording a secret by reference, the Verified stamp and its four methods, Recheck assertions, evidence tiers (what sets a stamp's lease) and high blast radius, the verified floor, CONFLICTED and UNCORROBORATED status markers, blocking contradictions, and promotion from repo tier to general tier.
memory_type: semantic
domain: knowledge-architecture
scope: general
verified: 2026-11-21
metadata:
  type: fact
  node_type: memory
  created: 2026-11-20
tags: [knowledge-architecture, contract]
keywords: [contract, CONVENTIONS, provenance, Source, section, backlink, sources, documents, source stub, citing an artifact, repo-qualified reference, secret by reference, redact, Verified, currency stamp, method, query, integration-test, doc-review, manual, by: agent, Recheck, kb-verify, evidence tier, asserted stamp, unasserted stamp, unasserted_ttl, check-stamps retired, trust tier, agent TTL, high blast radius, absent is not fresh, verified floor, unknown, CONFLICTED, UNCORROBORATED, since, narrowest unit, contradictions register, blocking, promotion, circular promotion, atomic promotion]
---
# Contract: provenance, currency, contradictions and promotion

Section numbers written as § are the contract's own, in `ai-kb:CONVENTIONS.md`; the contract stays the authority and this file restates its rules with provenance, copying their values and qualifiers. Why these rules exist is in [[design-thesis]], [[agent-lifecycle-roles]] and [[secret-governance]]; layout is in [[contract-layout-and-naming]]; retrieval is in [[contract-retrieval-and-verification]].

## Provenance is per section, and per fact on exception

- **Granularity: per-section, per-fact on exception.** Each topic block/heading carries a
  `Source: [[slug]]` backlink. A fact whose origin differs from its section gets its own inline
  `[[slug]]` backlink.
- **What counts as a section.** Every heading with text under it in a fact file. Three are
  exempt: the intro under the file's `#` title, which orients and holds no facts (put a fact
  under a heading, never in the intro); a heading with only subheadings under it; and a
  `CONFLICTED` section, which is not served (§8). Routers, the golden set, the queue, the state
  leaves (§2), episodic notes and source stubs are records, not fact files. `check-kb` fails a
  section with no `Source:` line, from contract 2.5. An unsourced section is unchecked, and the
  `verified:` floor never sees it.

Source: [[conventions-contract-rules]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'Each topic block/heading carries a' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## Stacking sources: dedup, corroboration, and one source backing two claims

- **Dedup merges provenance.** When two copies of a fact collapse into one canonical home, keep
  **all** contributing `[[source]]` links — never drop a citation in a merge.
- **Corroboration keeps its own stamp.** When a second source confirms a claim the KB already holds,
  it is recorded as its own `Source:` line with its own `Verified:` line beneath it — the pairs
  stack, they do not merge. Two sources checked on different dates by different methods are two
  pieces of evidence, and flattening them into one stamp discards which was verified when.
  (Conflicting sources are a different case entirely — §8.)
- **One source may back two claims in a section.** The same source can appear on more than one
  `Source:` line in a section when each line backs a different claim, each with its own `Verified:`
  stamp (and its own `Recheck:`, if it has one). The stale and unstamped-fact locus is
  `<heading>|<source>`, so those stamps share one ID: one queue record covers them all, and resolving
  it means re-checking, and re-stamping, each one.

Source: [[conventions-contract-rules]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'Two sources checked on different dates by different methods are two' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## Sources are backlinks, and a source stub records four things

- **Sources are `[[slug]]` backlinks.** Episodic notes already *are* sources — link straight to them
  (`[[YYYY-MM-DD-domain-notes]]`). An external artifact with no home in the KB (a spec, vendor page, PDF)
  gets a lightweight stub under the library's **`sources/`** so its backlink resolves. Don't create a
  stub for something already in the KB.
- A source stub (`sources/<slug>.md`, inside the library) records: title, origin (URL / page-ID /
  filename), date ingested, and where the artifact itself lives.

A source a script cannot check carries `- **Liveness:** manual` in its stub; that rule, from contract 2.4, is in [[releases-and-contract-versioning]] and [[agent-lifecycle-roles]].

Source: [[conventions-contract-rules]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'gets a lightweight stub under the library' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## sources holds citation stubs and documents holds archived artifacts

- **`sources/` and `documents/` hold different things.** `sources/` holds **only `.md` citation
  stubs**: routed from the library's `INDEX.md`, checked like any other file, and what a `Source:`
  line cites. `documents/` holds **the archived artifacts themselves**, kept verbatim: a schema dump,
  an ERD export, a PDF, a copied document. It is not routed (§3). A raw artifact never goes in
  `sources/`; it goes in `documents/`, and a stub in `sources/` points at it. `check-kb` fails a
  non-`.md` file under `sources/` from contract 2.1, and warns about one below it (§14).

Source: [[conventions-contract-rules]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'A raw artifact never goes in' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## Citing an artifact: inside the library, a stable outside page, or another repository

**Citing an artifact — three cases, and only one is hard.**

1. **Inside this library.** A `[[slug]]` or a relative path. Nothing special.
2. **Never in any repository, and stable where it is** — a vendor page, a published spec, a PDF at a
   permanent URL. The stub records *origin* (URL, page ID, publisher, date ingested) and no
   filesystem path, because there is not one.
   **A local file or export that would not survive elsewhere** — a schema dump, an ERD export, a
   pasted document, a page that may move — is archived under `documents/`, and its stub records the
   origin *and* the archived path. Evidence that exists on one machine resolves on no other.
3. **In another repository.** Two rules, in this order:
   - **Evidence travels with the domain it was folded from.** Archive the artifact inside the
     library, under `documents/`. This is **not** a single-canonical-home violation: the rule is
     one home per *fact*, not per *document*. A fact stated twice drifts because someone edits a
     copy; an archived artifact does not, because if it changes it is a new artifact with a new
     ingestion date. A library that carries its own evidence resolves every citation on any machine.
   - **What genuinely cannot travel gets a repo-qualified reference** — `ai-kb:CONVENTIONS.md`,
     `<repo>:<path-within-repo>`. **Never a relative path across a repository boundary**: that
     resolves only while two repositories happen to sit in the expected layout, and fails silently
     everywhere else. The qualified form degrades honestly — without the other repository you still
     know exactly what was cited and where it lives, you simply cannot open it from here.

Source: [[conventions-contract-rules]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'Never a relative path across a repository boundary' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## Provenance rots: a dead source is queued with its dependants, never edited

- **Provenance rots.** `check-kb` covers internal `[[slug]]`s, not external `Source:` URLs. External
  URLs get **HTTP liveness checks** (`kb-sources`: HEAD, then GET on any HTTP error but 429), run only when the person running it
  approves the network access; a dead URL is **queued** to the library's `needs-attention.md` with
  its dependants, never edited and never marked `provenance: BROKEN` by the tool. A 404 or 410 is an
  alert, not silence — and "unreachable" (DNS, timeout, 5xx, 403) is neither "dead" nor "unchanged".
  A person triaging a dead source may write `provenance: BROKEN` on the dependants; the hygiene sweep
  treats it as **unverified**.

How the liveness check requests and classifies a URL is in [[agent-lifecycle-roles]].

Source: [[conventions-contract-rules]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'A person triaging a dead source may write' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## A security finding records where a secret is and what to do, never what it is

Why the rule exists, and what the scan can and cannot see, is in [[secret-governance]]. The rule as the contract states it:

**A finding records where a secret is and what to do about it — never what it is.** A security
finding states the secret's **reference** (vault key or secret name), its **location** (file,
commit) and the **required action** (rotate, revoke) — never the literal value. The same holds for
any live credential: a knowledge base that quotes a password as evidence has leaked it through the
front door, as documentation. Correct shape:

> **Finding:** plaintext vendor credential committed in `PostToVendor.cs`. Secret ref:
> `kv://<vault>/vendor-password`. Present in history at `4c1a9e7`. **Action:** rotate in the vendor
> system — removal from code does not remediate.

No value, full traceability, actionable. A source artifact that contains a live credential is
**redacted before it is archived** under `documents/` — the archive is a copy, and a copy leaks.
A raw artifact archived there (a schema dump, an export, anything not `.md`) is scanned without
the bare-entropy rule, whose long-identifier hits would bury it; the credential patterns, the labelled
rule and the PII pass still read every line. Archive text as **UTF-8**: UTF-16 reads as binary to
every pass, so `check-secrets` blocks it as unscanned until it is converted.
**Personal identifiers follow the same shape:** a finding names the role or account type ("the
service account", "a former contractor"), not the colleague — no names, personal emails or personal
accounts. `check-secrets` (credential patterns, entropy, PII passes) is the net behind this rule, not
a substitute for it; it deliberately leaves names to the human gate.

Source: [[conventions-contract-rules]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'never the literal value.' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## A currency stamp: its format, its placement and the four methods

Provenance says where a fact came from. Currency says whether it is still true. They are different
questions and they attach at the same granularity (§4).

Each section that carries a `Source:` may carry a stamp directly beneath it, at the start of the line —
an indented stamp or `Recheck:` (under a list item, say) is never read, so `check-kb` rejects it:

```
Verified: 2026-09-19 · by: agent · method: query
```

- **`method:`** is one of `query` (re-run against the live system), `integration-test` (a real transaction
  against staging — the only thing that catches a vendor changing runtime behavior without touching its
  docs), `doc-review` (external source re-read), or `manual`.

Source: [[conventions-contract-rules]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF '(external source re-read), or `manual`.' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## Evidence, not just a date: what a re-stamp records and what by: can prove

- **Evidence, not just a date.** Every re-stamp records the **exact check run, the observed value, and the
  expected value** in a companion record. A stamp whose check queried the wrong source is worse than no
  stamp. **No evidence, no re-stamp.**
- **`by:` is a claim, and only partly checkable.** The original rule here said it was derived from the
  git committer identity and that CI rejected `by: human` from an agent identity. That is
  unimplementable, and the reason is worth stating rather than hiding: **an agent commits under the
  human's own git identity**, so there are not two identities to tell apart. `check-stamps`, which
  refused a commit announcing agent co-authorship that also added a `by: human` stamp, is retired
  (ADR-0022): the lease follows
  the stamp's evidence, not its `by:`, so a false `by: human` buys nothing.

Superseded: until engine v0.5.0 this bullet said `check-stamps` enforced a narrow guard and a human stamp bought "**ten times**" an agent stamp's lease; ADR-0022 retired both (the retirement is in [[script-reference-checks]] § check-stamps).

Source: [[conventions-contract-rules]] · [[adr-0022-evidence-not-identity-sets-the-lease]] · [[engine-changelog-v0.6.0]] (the `check-stamps` entry)
Verified: 2026-11-24 · by: agent · method: doc-review
Recheck: grep -qF 'refused a commit announcing agent co-authorship that also added a `by: human` stamp, is retired' "$KB_ENGINE_ROOT/CONVENTIONS.md" && [ ! -e "$KB_ENGINE_ROOT/scripts/check-stamps" ]

## A Recheck assertion: what it is, where it runs and how it must parse

- **A section may carry a re-check assertion**, directly beneath its stamp:

  ```
  Recheck: env -i HOME=$HOME PATH=/usr/bin:/bin bash -lc '[ -z "${KB_ENGINE_ROOT:-}" ]'
  ```

  It is an **assertion, not a query**: exit 0 means the fact still holds, non-zero means it no longer
  does. `kb-verify` runs these, so the evidence rule above is satisfied mechanically — the exact check
  is *in the file*, beside the claim it defends, and is reviewable in the diff that introduced it.
  Write one only where a command can actually decide the claim; most facts cannot be settled by a
  shell and should carry none. **An assertion that cannot fail is worse than none**: it manufactures
  freshness on a schedule.

  An assertion runs from its fact file's directory with `KB_ENGINE_ROOT` set to the engine running
  it. One that tests an engine script names it as `"$KB_ENGINE_ROOT/scripts/<name>"`, never by a
  relative path out of the library: a library can be registered anywhere (§2), so `../../scripts`
  holds only in the default layout and elsewhere fails as though the fact were stale. `kb-verify`
  reports such a path on every run.

  The command must parse: `check-kb` runs `/bin/sh -n -c` on it (the shell `kb-verify` runs it under)
  and fails one the shell rejects, at every contract version. The assertion is one line, so a quoted
  string cannot span lines.

The `Recheck (attended):` form, and how a scheduled `kb-verify --unattended` run treats it, are in [[releases-and-contract-versioning]] and [[agent-lifecycle-roles]]. That a failing assertion is reported and never applied, and that commands are printed and approved per run, are in [[agent-lifecycle-roles]].

Source: [[conventions-contract-rules]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'exit 0 means the fact still holds, non-zero means it no longer' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## What a doc-review Recheck must grep, and what kb-verify reports about weak ones

  A `doc-review` assertion that fetches a page must grep **the sentence that states this section's
  claim** — not the page's title, its subject or an identifier (`DENY` on the DENY page passes
  while the page exists, which a source-liveness check already covers), and not one landing-page
  line reused across sections: a page cannot state twenty different specific facts in one
  sentence, so one fragment cannot defend them. A stamp the assertion backs is no stronger than
  the assertion. `kb-verify` reports, on every run, a fetched fragment of four words or fewer and
  a (url, fragment) pair backing more than two sections or sections in more than one file. A short
  fragment that binds two runs of text, split by markup or by regex outside `grep -F` (a field and
  its value, say `Story Points</a> (Agile)`), tests a relation rather than a title and is not
  reported; an alternation counts as its weakest branch.

Source: [[conventions-contract-rules]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'a page cannot state twenty different specific facts in one' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## Evidence tiers: what sets a stamp's lease, and what makes a fact high blast radius

- **Evidence tiers.** The evidence behind a stamp sets its lease; who wrote it does not
  (ADR-0022). An **asserted
  stamp**, one backed by a passing `Recheck:`, attended or not, stays fresh for the full
  `freshness_horizon`. **Passing** means the assertion lint above does not flag it and the library's
  `needs-attention.md` holds no open failed-recheck record raised against the section's current
  stamp. Such a record has `category: stale-recheck` and `status: open`, its `file:` and `section:`
  name the section, and its `stamp:` equals the stamp's content after `Verified:` (`<date> · by:
  <by> · method: <method>`, as `kb-verify` writes it). A failed check is reported and never applied
  (above), so the record is how the failure reaches the stamp: the section is unasserted, on the
  short lease and out of assertion coverage. It stays so until a newer stamp replaces the one the
  record was raised against, such as a passing attended `kb-verify --apply`, which makes it asserted
  again at once. The record itself stays open as the Arbiter's decision, and nothing in the engine
  closes one. A record with no `stamp:` (an older one) covers a stamp dated on or before the
  record's `date:`. A `stale-recheck-error` (a timeout, or a command that could not run), an
  unreadable queue and an unreadable record date demote nothing. Every other stamp is an **unasserted
  stamp**, someone's word, and stays fresh for a tenth of the horizon, whether `by:` says `human` or
  `agent`. The lint costs tier: an assertion it flags is demoted to unasserted, which stays a warning
  for the sweep and now has a consequence. A domain may set the short lease with `unasserted_ttl`
  (§3), which must stay shorter than the horizon; declare it when unasserted stamps would otherwise
  expire before the next sweep. Do not stretch the horizon instead: that stretches the asserted
  lease with it. The Arbiter's work is to approve the checks, decide failed checks and contradictions
  (§8), and own attended decisions, not to re-stamp facts. `by:` and `method:` stay on the stamp as a
  record and are never weighed.
  **High-blast-radius** facts follow the same evidence rules, and are gated by consequence: **an
  attended decision is required on any change to one, and on any restamp after a failed check.**
  Deploy targets, environment routing and credential references are the usual cases.
  **The test is the consequence, not the
  subject matter — if this fact is wrong, what happens?** Damage that is expensive or impossible to
  undo (shipped to the wrong place, written to the wrong environment, a wrong credential used) is
  high blast radius. A skipped check, a confused reader or a wasted afternoon is not.

  A fact is not high-blast-radius merely because it *mentions* an environment variable, a config file
  or a hook: `config-resolution.md` is about how variables propagate, and getting it wrong misleads a
  developer rather than deploying anything anywhere. Read the category wide enough and every
  agent-written fact is permanently unverifiable — a queue full on the day it is created, which
  nobody reads twice. Both first sweeps read it that way and filed 11 such items; the category was
  underspecified, and this paragraph is the repair.

Superseded: until engine v0.5.0 this bullet was "Trust tiers" (agent TTL "**1/10th**" of the horizon, `agent_ttl` to change it, human re-verification for a high-blast-radius fact); ADR-0022 replaced it with evidence tiers, and contract 2.7 renames the field (see [[releases-and-contract-versioning]]). Superseded in part: until #264 "passing" only meant the lint did not flag the assertion.

Source: [[conventions-contract-rules]] · [[adr-0022-evidence-not-identity-sets-the-lease]] · [[engine-changelog-v0.6.0]] (#264) · [[adr-0005-currency-cadence]] (its trust tiers, superseded)
Verified: 2026-11-24 · by: agent · method: doc-review
Recheck: grep -qF '`freshness_horizon`. **Passing** means the assertion lint above does not flag it and the library'"'"'s' "$KB_ENGINE_ROOT/CONVENTIONS.md" && grep -qF 'stamp**, someone'"'"'s word, and stays fresh for a tenth of the horizon, whether `by:` says `human` or' "$KB_ENGINE_ROOT/CONVENTIONS.md" && grep -qF '**High-blast-radius** facts follow the same evidence rules, and are gated by consequence: **an' "$KB_ENGINE_ROOT/CONVENTIONS.md" && ! grep -qF 'Agent-verified TTL is materially shorter' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## Absent is not fresh, and the verified floor over nothing or over a gap

- **Absent ≠ fresh.** A section with no stamp is `unknown`, and ranks **ahead of a merely stale one**
  in the attention queue — absence hides more easily than age. "Ahead of stale" is the claim, not
  "ahead of everything": a contradiction being actively served outranks both, and the full ordering
  belongs to the sweep that builds the queue.
  a sweep cannot flag what was never written.
- **A floor over nothing is `unknown`.** If no section in the file carries a stamp — every claim in
  it became `CONFLICTED`, or none was ever stamped — the frontmatter reads `verified: unknown`. It
  never keeps a date inherited from before the stamps went away: that date now certifies nothing and
  reads as freshness the file does not have.
- **File frontmatter carries `verified:` as the floor** — the oldest stamp among the file's sections
  that are not `CONFLICTED` (a disputed section is not served, so its stamp certifies nothing a reader
  can get), derived and lint-checked. Sweeps grep one field; the sections hold the truth.
- **A floor over a gap is `unknown` too.** If any section that is not `CONFLICTED` cites a `Source:`
  and carries no stamp, the floor is `unknown`, however recent the other stamps: the oldest stamp
  among the stamped sections says nothing about the one nobody checked. `check-kb` derives the floor
  this way and fails a declared one that disagrees, or a stamp dated in the future; `kb-verify
  --apply` moves the floor when it restamps.

Source: [[conventions-contract-rules]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'in the attention queue — absence hides more easily than age.' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## CONFLICTED: a disputed section is unretrievable, and the contradictions register

A register beside the facts is not a control: **the retrieval path never reads it**, so an agent still
serves one of the conflicting values with confidence. When two sources disagree and you cannot verify
which is right:

- do **not** silently pick a winner, and do **not** edit a subordinate source of truth to match;
- mark the conflicting section **inline** with `status: CONFLICTED`, **dated on the line it is raised**
  (`status: CONFLICTED · since: YYYY-MM-DD`), stating both claims, the risk of each, and how to
  verify. The date is not decoration: a contradiction's age drives its escalation, and nothing else
  in the file records when the dispute began;
- **retrieval agents refuse to serve a `CONFLICTED` section** — they surface the conflict instead. The
  golden set (§10) carries an UNRESOLVED case proving this behavior;

- register a one-line flag + pointer in `<domain>-contradictions.md`, routed from the library's
  `INDEX.md` like any state leaf (§3); the canonical note stays in the file that owns the topic.

Source: [[conventions-contract-rules]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'they surface the conflict instead.' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## Marking a dispute: the narrowest unit, and a status marker applies to its own heading only

- **mark the narrowest unit that holds the disputed claim.** A `CONFLICTED` section is refused
  *wholesale*, so a single disputed value sitting among sound facts takes its neighbours down with
  it — an agent goes blind to correct knowledge because something nearby is in doubt. If a disputed
  claim shares a section with undisputed ones, **split it into its own subsection** so only the
  claim in doubt is withheld. Collateral refusal is a bug, not an abundance of caution;
- **a status marker applies to the heading it sits directly under — never to the headings enclosing
  it.** A marker under `### Default sample size` withholds that subsection; the rest of the `##`
  section above it stays retrievable. When you split a claim out, add a `positive` golden case for a
  sibling fact left behind, so the eval proves the neighbours are still served;

Source: [[conventions-contract-rules]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'Collateral refusal is a bug, not an abundance of caution;' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## UNCORROBORATED: one source the authority does not back

**One source, unconfirmed by the authority: `UNCORROBORATED`.** A value stated by a single source
that is not the authority for it (a community post, say, for a figure the vendor never published) is
not a contradiction. There is no second claim, and marking it `CONFLICTED` makes retrieval refuse it
and forces someone to invent "the other side". Left as plain text with a caveat in its prose, it is
served as settled, and the caveat may never reach the answer. Instead:

- mark the section **inline** with `status: UNCORROBORATED · since: YYYY-MM-DD`, under the narrowest
  heading that holds the claim (the placement rule above applies to every status marker). Name the
  single source, and say what would confirm it: which authority, and where it would say so;
- **retrieval serves it, caveat first.** The answer begins `uncorroborated — single source:`, then
  gives the value, quoted, and names the source. The golden set carries an `uncorroborated` case
  proving it, wherever the library holds such a section (§10);
- the section is **served**, so its stamp counts toward the file's `verified:` floor, and an unstamped
  one counts as unstamped (§7). Its currency still matters: the single source can change too;
- **it clears when the authority speaks.** If the authority confirms the value, remove the marker
  and cite the authority. If it states another value, that is a correction or a contradiction, not
  an uncorroborated claim. `check-blast` flags a removed marker for review, as it does a removed
  `CONFLICTED` one;
- **it is not a hedge.** A claim the file itself infers or has not tested is a hedge, served under
  `Not established in the knowledge base` (§10). `UNCORROBORATED` is a claim a source does state,
  where the weakness is the source's authority, not the reasoning.

Source: [[conventions-contract-rules]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'A value stated by a single source' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## Blocking and informational contradictions, and why moves of a load-bearing fact are verbatim

**Blocking vs. informational.** A contradiction on a fact actively needed is *blocking* and names who
resolves it and via what artifact. A blocking contradiction **must be cleared before its domain is
promoted to the general tier** (§9). Flagging forever just accretes.

**Moves of a load-bearing fact are verbatim** — paraphrasing during a reshape can manufacture a new
contradiction. An LLM fold can round a constant, drop a qualifier, or merge two behaviors into one
generalization: load-bearing values are **copy-pasted**, and each extracted fact is **human-diffed against
its source** before commit.

Source: [[conventions-contract-rules]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'Flagging forever just accretes.' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## Promotion from repo tier to general tier: two guards, never automated

Knowledge usually *starts* repo-specific and later proves general. Promotion moves the fact to the general
root and leaves a backlink at the origin. Two guards:

- **No circular promotion.** The confirming evidence must be **external to the KB** — a live vendor URL
  with excerpt, a direct system result, or a named human attestation with external citation. If the
  evidence is *another KB file*, the promotion is **rejected**. The KB may never be its own evidence for
  general-tier truth. One observation is not a general truth.
- **Atomic promotion.** The general-root add and the origin-stub update are a **single paired commit** —
  otherwise the dual-root resolver finds both the promoted copy and the full pre-promotion copy in the
  gap, silently undoing the dedup. The origin becomes an **empty redirect stub** that cannot answer
  queries: agents follow the link, never serve stub content.

Promotion is **never automated**. Neither is resolving a contradiction, nor deleting knowledge.

ADR-0004 states the same two guards; see [[adr-0004-scope-attribute]].

Source: [[conventions-contract-rules]] · [[adr-0004-scope-attribute]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'The KB may never be its own' "$KB_ENGINE_ROOT/CONVENTIONS.md"
