---
name: contract-retrieval-and-verification
description: The contract rules for retrieval and for verifying a domain — what every retrieval agent is held to, the golden set (where it lives, the record fields including the optional expected_evidence, the five case types and when each is required, alt_excerpt, what an excerpt is), the two test tiers, and the section 13 tables of checks with what each proves.
memory_type: semantic
domain: knowledge-architecture
scope: general
verified: 2026-11-21
metadata:
  type: fact
  node_type: memory
  created: 2026-11-20
tags: [knowledge-architecture, contract]
keywords: [contract, CONVENTIONS, retrieval contract, golden set, case, positive, negative, unresolved, uncorroborated, cross-root, expected_file, expected_excerpt, alt_excerpt, excerpt, markup-free, whitespace, check-golden, grade-eval, grade-routing, routing check, answer-grounding eval, embedded-fact lint, verifying a domain, checks, check-kb, check-scope, check-xlinks, check-stamps, check-blast, kb-verify, kb-sources, kb-watch, kb-due, kb-owner, check-tool-scope, check-exec-bits, check-secrets, regression suite]
---
# Contract: retrieval, the golden set and verifying a domain

Section numbers written as § are the contract's own, in `ai-kb:CONVENTIONS.md`; the contract stays the authority and this file restates its rules with provenance, copying their values and qualifiers. How the retrieval agent behaves is in [[retrieval-agent-behavior]]; why an eval can mislead is in [[retrieval-eval-integrity]]; layout is in [[contract-layout-and-naming]]; provenance and currency are in [[contract-provenance-and-currency]].

## Citing, quoting and loading: what every retrieval agent is held to

Every retrieval agent's body states, and every agent is held to:

> *Your knowledge lives in the libraries registered on this machine, and in the project's own
> `knowledge/` tree. Load INDEX, descend to the relevant file(s), cite them, then act. Do not answer
> from memory.*

- **Cite every file used.** "file(s)" is plural deliberately: an answer may need a general-tier fact *and*
  a repo-tier fact. State when an answer is **partial**. A citation means *the answer rests on this
  file*, so a refusal cites nothing: the files checked to establish an absence are named in the answer
  text, never cited. `grade-eval` holds a negative case to exactly this.
- **Quote what the answer rests on.** Each value the answer depends on comes with the sentence that
  states it, quoted word for word from the cited file (markup dropped, words kept). The reader can
  find the quote in the file, and `grade-eval` can tell it from a paraphrase of recall.
- **Load only the libraries the question touches.** A question about one domain must not pay for
  every installed library; a question spanning a project fact and a library fact loads both.

Source: [[conventions-contract-rules]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'Each value the answer depends on comes with the sentence that' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## Absence, episodic notes, hedges, disputes, uncorroborated claims and the verified date

- **On absence:** answer *"fact not found — check KB"*. Never fill the gap from model memory — nor from
  the code, config or scripts around the knowledge base. Reading the source is the caller's job; a
  retrieval agent that reads it answers correctly and hides the gap.
- **On a fact held only in an episodic note:** the answer begins *"Not established in the knowledge
  base"*, never with yes or no, and reports observed and believed-but-untested material as such.
  A session note is a record; distillation decides what in it is a fact, and retrieval must not
  promote what distillation refused.
- **On a hedge in any file:** a claim the file itself marks untested, inferred or probable is reported
  as such and never leads the answer — a `Verified:` stamp beside it dates what was checked, not what
  was inferred. **Never chain facts from two files into a conclusion neither states.**
- **On `CONFLICTED`:** serve nothing (§8). The answer begins *"disputed — not served"*, states both
  claims and who makes each, and cites the file holding the conflict. That is the one refusal that
  cites, because the dispute itself is what the reader must be able to check.
- **On `UNCORROBORATED`:** serve it, caveat first (§8). The answer begins *"uncorroborated — single
  source:"*, gives the value and names the source.
- **Surface the `verified:` date** on freshness-sensitive facts.

Source: [[conventions-contract-rules]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'cites the file holding the conflict.' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## The golden set: where it lives, why it is not routed, and what a record carries

**The golden set** lives at the library root, `<domain>-golden.md`, and travels with the domain it
tests. The `-golden` suffix is load-bearing: a **folder name is reserved** (§5), so a file named
`<domain>.md` would collide with the library's own directory name. It is **not routed from the
library's `INDEX.md`** — an agent that can descend to the oracle can read the answers, and its
refusals then prove nothing. **Eval evidence**, kept under `documents/eval-<date>/`, records past
answers to the same questions, and a retrieval agent is held to it exactly as it is to the golden
set: never opened, and excluded from every search. Each record carries:

```yaml
- case: positive | negative | unresolved | cross-root | uncorroborated
  question: <what is asked>
  expected_file: <the file that must answer it; a cross-root case may write <library>:<path>>
  expected_excerpt: <text that must appear, grep-verifiable against expected_file>
  alt_excerpt: <optional, repeatable: another sentence in expected_file stating the same fact>
  expected_evidence: <optional, positive cases only: check | word>
```

`case:` is required — a minimum of "≥2 negative cases" is unenforceable if nothing marks a case as
negative. For a `negative` case, `expected_file` may be empty and `expected_excerpt` holds the
refusal the agent must produce.

Every domain carries **≥2 negative cases** — a knowledge base that never refuses is one that guesses.

**`expected_evidence` tests how retrieval states what backs a fact** (ADR-0022). `check` means the section holding the excerpt carries a `Recheck:`, and the answer must say it "carries a check". `word` means the section is stamped and carries no `Recheck:`, and the answer must say "someone's word" before it gives the fact. A section with no stamp is a separate tier, `unstamped`, which retrieval reports as unstamped and not as someone's word, so `check-golden` fails `word` against it. `check-golden` fails a record whose value disagrees with the file, by reading the section that holds the excerpt (`kbengine.golden.evidence_of`). Retrieval cannot run the weak-assertion lint, so it says "carries a check", never "asserted". The field is optional and positive cases only. A fact without a check is still served: only a `CONFLICTED` fact is refused. The field is not a contract version: an optional golden-set field is "not a contract change, because no existing library can break" (§14).

Source: [[conventions-contract-rules]] · [[adr-0022-evidence-not-identity-sets-the-lease]] · [[engine-changelog-v0.6.0]] (#237) · [[script-check-golden]] · [[scripts-kbengine-package]] (`golden.evidence_of`)
Verified: 2026-11-24 · by: agent · method: doc-review
Recheck: grep -qF 'expected_evidence: <optional, positive cases only: check | word>' "$KB_ENGINE_ROOT/CONVENTIONS.md" && grep -qF '`word` means it is stamped but carries no `Recheck:`, and the answer must say "someone'"'"'s word" before' "$KB_ENGINE_ROOT/CONVENTIONS.md" && (t=$(mktemp -d); r=0; mk() { d="$t/$1"; mkdir -p "$d"; printf -- '---\nname: x-index\ndescription: d\nmemory_type: reference\ndomain: %s\nscope: general\nfreshness_horizon: 90d\nverifier_budget: 5\nsweep_interval: 30d\nlast_swept: never\n---\n# x\n\n- \133f](f.md) — f\n' "$1" > "$d/INDEX.md"; printf -- '---\nname: f\ndescription: d\nmemory_type: semantic\ndomain: %s\nscope: general\nverified: 2026-11-20\nmetadata:\n  type: fact\n  node_type: memory\n  created: 2026-01-01\ntags: [x]\nkeywords: [x]\n---\n# f\n\n## A\n\nThe sky is blue.\n\nSource: \133\133f]]\n%b' "$1" "$3" > "$d/f.md"; printf -- '---\nname: %s-golden\ndescription: d\nmemory_type: reference\ndomain: %s\nscope: general\nmetadata:\n  type: reference\n  node_type: golden-set\n  created: 2026-01-01\ntags: [x]\nkeywords: [x]\n---\n# g\n\nUnresolved case: not applicable.\n\n```yaml\n- case: positive\n  question: q\n  expected_file: f.md\n  expected_excerpt: The sky is blue.\n  expected_evidence: %s\n- case: negative\n  question: n1\n  expected_file:\n  expected_excerpt: fact not found — check KB\n- case: negative\n  question: n2\n  expected_file:\n  expected_excerpt: fact not found — check KB\n```\n' "$1" "$1" "$2" > "$d/$1-golden.md"; }; V='Verified: 2026-11-20 · by: agent · method: manual\n'; mk fxg check "${V}Recheck: true\n"; mk fxh word "${V}Recheck: true\n"; mk fxs word "$V"; mk fxu word ''; g="$KB_ENGINE_ROOT/scripts/check-golden"; "$g" "$t/fxg" >/dev/null 2>&1 || r=1; "$g" "$t/fxs" >/dev/null 2>&1 || r=1; "$g" "$t/fxh" > "$t/h" 2>&1; [ $? -eq 1 ] && grep -qF "expected_evidence 'word' but the section holding the excerpt in f.md carries a Recheck:" "$t/h" || r=1; "$g" "$t/fxu" > "$t/u" 2>&1; [ $? -eq 1 ] && grep -qF 'has no Verified: stamp, so retrieval reports it unstamped, not as someone' "$t/u" || r=1; rm -rf "$t"; exit $r)

Source: [[conventions-contract-rules]]
Verified: 2026-11-24 · by: agent · method: doc-review
Recheck: grep -qF 'an agent that can descend to the oracle can read the answers, and its' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## Unresolved and uncorroborated cases: what each names and what check-golden and grade-eval require

An **`unresolved` case** names the file holding the `CONFLICTED` section as `expected_file`, and its
`expected_excerpt` is exactly the refusal, `disputed — not served`. No fact file contains that
phrase, so `check-golden` checks something else: that the named file really holds a `CONFLICTED`
section, so the case tests a dispute that exists. `grade-eval` requires the phrase in the answer and
the file among its citations.

An **`uncorroborated` case names the file holding the `UNCORROBORATED` section**, and its
`expected_excerpt` is the marked claim. `check-golden` checks that the excerpt sits in a section
carrying the marker, so the case tests the claim that is marked and not a sound neighbour.
`grade-eval` requires the excerpt in the answer, the file among its citations, and the answer to begin
`uncorroborated — single source`. **It is required wherever the library holds such a section**, and
not otherwise: `check-golden` fails a library holding the marker with no such case.

Source: [[conventions-contract-rules]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'checks something else: that the named file really holds a' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## Cross-root cases, and when an unresolved or cross-root case is required

A **`cross-root` case names the file in the other library that answers it**, written
`<library>:<path>`, for example `agile-requirements:elicitation-techniques.md`, and its excerpt is
the sentence in that file. The library is another registered library. `check-golden` checks the
excerpt in that file and that the other library's routers reach it. `grade-routing` and `grade-eval`
then require the agent to read and cite it. The agent also has to read content in the case's own
library, where the hop starts. Only a `cross-root` case may name another library's file. A library
checked on its own, as in its CI, cannot see its siblings, so `check-golden` reports such a record as
not checked there. It is checked where the libraries are installed together. The older form, a file
in the own library with the `[[library:slug]]` link text as `expected_excerpt`, still passes, but it
only checks where the hop starts, not the file that answers.

An **`unresolved` case is required only where the domain actually holds a `CONFLICTED` section**, and a
**cross-root case only where the domain actually links across libraries** — a domain with no contradiction and no
cross-library link cannot have either without fabricating one, and a golden case invented to satisfy a
count is worse than an absent one — it tests a fiction and reports success. Where either does not
apply, say so in the file, with the reason.

Source: [[conventions-contract-rules]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'Only a `cross-root` case may name another library' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## alt_excerpt: when a file states a fact twice, and the rules for an alternative

**`alt_excerpt` exists because a file may state one fact twice.** This is usually a summary line and
the source passage it quotes. An agent that quotes either sentence has retrieved the fact, but an
oracle naming only one fails it. Rules:

- **Every alternative is held to the main excerpt's standard.** It must be present in
  `expected_file` (`check-golden`), and it must state the *same* fact, not a neighbouring one. An
  answer passes if it quotes any listed excerpt. The grader checks that every one is still in the
  file.
- **Refusal cases take none.** A `negative` or `unresolved` excerpt is a fixed phrase, with no
  second way to say it.
- **An alternative added after results were seen is an amendment.** Record it beside the record,
  and in the eval evidence it was prompted by, as the date and the run that prompted it. Stored
  grades are never re-stated under the amended oracle.

Why an oracle that follows the agent measures nothing is in [[retrieval-eval-integrity]].

Source: [[conventions-contract-rules]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'A `negative` or `unresolved` excerpt is a fixed phrase, with no' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## What an excerpt is: one sentence, markup-free, whitespace-normalised, updated with a rename

**An excerpt is the one sentence, or the one clause, that carries the value**: never a passage
of several sentences, and never a lone word. A lone word ("label") is passed by an answer that was
guessed. A passage of several sentences fails an answer that quoted only the one that mattered.

**Excerpts are matched with whitespace normalised**, so an excerpt may span a hard-wrapped line in the
source. It may **not** contain markdown markup: emphasis exists in the file and never in a spoken
answer, so an excerpt carrying it tests formatting rather than grounding. The golden set updates **in the same commit** as any
file rename or split — an `kb-organize-domain` exit criterion — or the oracle rots.

Source: [[conventions-contract-rules]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'A lone word ("label") is passed by an answer that was' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## The two tiers that test the golden set, and the embedded-fact lint that is the actual lock

Two tiers test it, and they are not interchangeable:

- **Routing check**, in two tiers. The deterministic tier (`check-golden`, every commit, no LLM): the
  expected file exists, holds the excerpt, and is reachable from the root `INDEX.md` through the
  routers. The agent tier (`grade-routing`, sampled and started by a person): the agent's tool calls
  show it Read the expected file, read content from no library the case does not need, read nothing
  outside the registered libraries but this contract, and never touch the golden set. It is graded from the transcript, not from what the agent says it did.
- **Answer-grounding eval** (sampled, started by a person): the agent runs; its answer must contain the
  `expected_excerpt`, **verified by grep against the expected file** — not by an LLM judge. This is what
  catches answering from memory. `grade-eval --transcripts` grades the agents' transcripts directly,
  through the same mapping file as `grade-routing`.

The golden set is a fire alarm on fixed questions. The **embedded-fact lint** over agent and command
files — flagging identifiers, numeric constants, environment-specific values and credential-shaped
strings — is the actual lock. An inlined schema routes correctly and passes every eval.

Source: [[conventions-contract-rules]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'The golden set is a fire alarm on fixed questions.' "$KB_ENGINE_ROOT/CONVENTIONS.md"

## When a domain is done: the rule behind the list of checks

A domain is not "done" until these pass clean. The full rule set `check-scope` owes, marked built or
to-build, is in [[adr-0004-scope-attribute]].

Section 13 says "All twenty-seven are built." and its tables list twenty-seven checks, each marked built. A library passing the built checks is **structurally sound within itself**
— it is not verified. Whether an agent retrieves rather than answering from memory is what the
routing check and the answer-grounding eval are for: both are built, and routing is graded from the agent's tool calls rather than its own account of them.

The count is the table's: it lost the stamp-honesty row when `check-stamps` was retired (ADR-0022).

Source: [[conventions-contract-rules]] · [[adr-0004-scope-attribute]] · [[adr-0022-evidence-not-identity-sets-the-lease]]
Verified: 2026-11-24 · by: agent · method: doc-review
Recheck: grep -qF 'All twenty-seven are built.' "$KB_ENGINE_ROOT/CONVENTIONS.md" || exit 1; t=$(awk '/^## 13\. /,/^## 14\. /' "$KB_ENGINE_ROOT/CONVENTIONS.md"); n=$(printf '%s\n' "$t" | grep -c '^| .*\*\*built\*\*'); rows=$(printf '%s\n' "$t" | grep -c '^| '); [ "$n" -eq 27 ] && [ "$rows" -eq 28 ]

### Structural checks: frontmatter, links, slugs, orphans, sources, contract version, stamps and sections

| Check | What it proves | Status |
|---|---|---|
| frontmatter lint | required fields present, `name` matches filename | **built** — `check-kb` |
| link check | `[[slug]]` and router links resolve **within one library** | **built** — `check-kb` |
| slug uniqueness | no filename collision, and no file colliding with a folder name | **built** — `check-kb` |
| orphans | every file reachable from the root `INDEX.md` by router links; golden set and `documents/` exempt (§3) | **built** — `check-kb` |
| raw artifacts in `sources/` | `sources/` holds only `.md` citation stubs; an artifact is archived under `documents/` (§6). Fails from contract 2.1, warns below it (§14) | **built** — `check-kb` |
| contract version | `kb_contract` is a quoted `MAJOR.MINOR`; rules apply by it; a library behind is told what newer versions add, one ahead by a minor is checked by the newest rules known, and an unsupported major fails and is not served (§14) | **built** — `check-kb`, `kb-libraries`, `kb-due` |
| stamp placement | no indented `Verified:` / `Recheck:` line outside a code fence (§7) | **built** — `check-kb` |
| re-check syntax | a `Recheck:` / `Recheck (attended):` command parses under `/bin/sh -n`, at every contract version (§7) | **built** — `check-kb` |
| `verified:` floor | a declared floor equals the one derived from the section stamps; no stamp dated in the future (§7) | **built** — `check-kb` |
| section provenance | every section with content in a fact file carries a `Source:` line; the intro, parent headings and `CONFLICTED` sections are exempt (§6). Fails from contract 2.5, warns below it (§14) | **built** — `check-kb`, `kb-due` |

Source: [[conventions-contract-rules]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF '| frontmatter lint | required fields present, `name` matches filename |' "$KB_ENGINE_ROOT/CONVENTIONS.md"

### Lint, scan and scope checks, and the routing and answer-grounding checks

| Check | What it proves | Status |
|---|---|---|
| embedded-fact lint | no facts inlined in the behaviour layer | **built** — `check-embedded-facts` |
| secret scan | staged, worktree, or every blob in full history | **built** — `check-secrets` |
| executable bits | hooks and scripts recorded 100755, so a clone is not silently unguarded | **built** — `check-exec-bits` |
| hygiene sweep | stale and missing stamps, ageing contradictions, golden-set rot, doubly-routed files | **built** — the `kb-audit` skill |
| `check-scope` | `scope:` values well-formed; every file agrees with its library's tier; no relative link climbing out of a library | **built** |
| `check-xlinks` | `[[library:slug]]` resolution from any tree — the registered libraries (`--installed`) and each project tree passed to it — into the registered libraries; dead links fail, absent libraries warn. A link in a code fence is an example and is skipped; one in an inline code span is resolved but never warns of an absent library. With `--installed`, a library from another origin than `links_to` names is a dependency mismatch, which warns, and a dead link in a read-only library warns (ADR-0020) | **built** |
| routing check | golden-set questions actually route to `expected_file`. Deterministic tier: the file exists, holds the excerpt and is reachable from the root `INDEX.md` through the routers. Agent tier: an agent's tool calls Read the expected file, read content from no library the case does not need, read nothing outside the registered libraries but this contract, and never touch the golden set or an eval folder. The agent tier is sampled and started by a person, never per commit ([[adr-0013-steady-state-surfaced-not-scheduled]]) | **built** — `check-golden` (every commit) and `grade-routing` (graded from a subagent's tool calls, not its self-report) |
| answer-grounding eval | the agent's answer contains `expected_excerpt` or an `alt_excerpt` AND the expected file is among those it cited (a cited path is resolved to a file, not string-matched); quote characters and trailing punctuation are normalised, wording is not; grep-verified | **built** — `grade-eval` |

Source: [[conventions-contract-rules]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF '| executable bits |' "$KB_ENGINE_ROOT/CONVENTIONS.md"

### Currency, ownership, drift and liveness tools, and the regression suite

| Check | What it proves | Status |
|---|---|---|
| re-check assertions | a stale section's `Recheck:` still exits 0; failures reported, never applied | **built** — `kb-verify` |
| assertions that cannot fail | a fetched fragment of ≤4 words, or one (url, fragment) backing >2 sections or several files; reported every run, never a failure (§7) | **built** — `kb-verify` |
| ownership resolution | who a finding belongs to, by CODEOWNERS' last-match-wins rule | **built** — `kb-owner` |
| high-blast gate | a diff that deletes a knowledge file, drops a `status: CONFLICTED` or `status: UNCORROBORATED` marker, or promotes a fact's scope toward `general` is flagged, for CI to hold until an owner labels it; detection only | **built** — `check-blast` |
| tool-scope declaration | each agent, skill and command declares a tool tier and is consistent with it; proves the declaration, not that a skill cannot use an undeclared tool | **built** — `check-tool-scope` |
| sweep and re-check due | which trees are due for a sweep, and how many facts, open queue items and episodic notes await a librarian; a reminder, exit 0 always | **built** — `kb-due` |
| external drift | a saved snapshot of an external source still matches its recorded baseline; drift is queued, never applied; offline, fetches nothing | **built** — `kb-watch` |
| guardrail regression suite | each check still fails on the defect it was written to catch | **built** — `tests/run-checks`, in CI and in the engine hook |
| external `Source:` liveness | the URLs source stubs cite still resolve (HEAD, then GET on any HTTP error but 429; GET decides); a 404/410 is queued as `source-dead` and an undecidable one, or a redirect to a sign-in page, as `source-unreachable`, each with its dependants; a `Liveness: manual` stub is counted, never fetched; fetches only with `--yes`, never edits or marks a fact | **built** — `kb-sources` |

Superseded: until engine v0.5.0 this table had a row `| stamp honesty | a commit claiming agent co-authorship does not also add a `by: human` stamp | **built** — `check-stamps` |`. `check-stamps` is retired (ADR-0022): the lease follows a stamp's evidence, so there is nothing for the check to refuse. The assertion lint row's "reported every run, never a failure" now sits beside a consequence, which is that a flagged assertion is demoted to an unasserted stamp ([[agent-lifecycle-roles]] § A stamp's lease follows its evidence, not its author).

Source: [[conventions-contract-rules]] · [[adr-0022-evidence-not-identity-sets-the-lease]]
Verified: 2026-11-24 · by: agent · method: doc-review
Recheck: ! grep -qF '| stamp honesty |' "$KB_ENGINE_ROOT/CONVENTIONS.md" && grep -qF '| re-check assertions | a stale section' "$KB_ENGINE_ROOT/CONVENTIONS.md" && [ ! -e "$KB_ENGINE_ROOT/scripts/check-stamps" ]
