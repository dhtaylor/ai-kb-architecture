---
name: skills-agents-and-commands
description: What ai-kb's behavior layer contains and what each piece is for, refuses and stops at — the two plugins (knowledge-tools, analysis-tools) and how a consumer registers them (ADR-0006), the tier each file declares, the six skills kb-ask, kb-update-domain, kb-create-domain, kb-distill, kb-organize-domain and kb-audit, the kb-retrieve agent, the /kb-capture and /kb-init commands, and the intelligence-analysis skill; which one to use rather than its neighbor, what it refuses, where it stops for a person, and which engine scripts it runs.
memory_type: semantic
domain: knowledge-architecture
scope: general
verified: 2026-11-21
metadata:
  type: fact
  node_type: memory
  created: 2026-11-20
tags: [knowledge-architecture, skills, agents, commands, plugins]
keywords: [plugin, marketplace, governed-knowledge, knowledge-tools, analysis-tools, ADR-0006, extraKnownMarketplaces, enabledPlugins, directory source, shadow, tier, curation, retrieval, triage, analysis, allowed-tools, kb-ask, kb-retrieve, kb-update-domain, kb-create-domain, kb-distill, kb-organize-domain, kb-audit, kb-capture, kb-init, intelligence-analysis, coaching log, refusals, review gate, read-only, Do NOT use]
---
# Skills, agents and commands

The engine's behavior layer is two plugins. Their skills, agent and commands hold no facts: they read
facts from the registered libraries at run time. This file says what each piece is for, which neighbor
to use instead, what it refuses, where it stops for a person and which engine scripts it runs. A
skill's own `SKILL.md` is the behavior and states every step; the steps are not restated here. How a
library holds knowledge is in [[design-thesis]]; how the retrieval agent decides where to look is in
[[retrieval-agent-behavior]].

## The two plugins and what each holds

The marketplace is named `governed-knowledge`, described as "The governed knowledge base and the
behaviour that curates and queries it." It lists two plugins, each at version `0.4.0`:

- **`knowledge-tools`**, category `knowledge`, source `./plugins/knowledge-tools`. It holds the skills
  `kb-ask`, `kb-audit`, `kb-create-domain`, `kb-distill`, `kb-organize-domain` and `kb-update-domain`,
  the agent `kb-retrieve`, and the commands `kb-capture` and `kb-init`. Its manifest says: "Skills
  curate domains under the conventions contract; agents and commands retrieve from them under the
  retrieval contract. Holds no facts — it reads them from the registered libraries at runtime."
- **`analysis-tools`**, category `analysis`, source `./plugins/analysis-tools`. It holds one skill,
  `intelligence-analysis`. Its manifest says it "Reviews a work product for analytic rigor and coaches
  intelligence-analysis tradecraft, retrieving every standard, technique, and bias from the
  intelligence-analysis knowledge library at runtime. Holds no facts of its own."

Source: [[plugin-manifests]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF '"source": "./plugins/analysis-tools",' "$KB_ENGINE_ROOT/.claude-plugin/marketplace.json"

## How a consumer registers the plugin: explicitly, by a directory source (ADR-0006)

ADR-0006 decided that the knowledge repository is "also a plugin marketplace. Consumers register it
explicitly." A consumer declares the marketplace and enables the plugin in its settings:

```json
{
  "extraKnownMarketplaces": {
    "governed-knowledge": { "source": { "source": "directory", "path": "./knowledge" } }
  },
  "enabledPlugins": { "knowledge-tools@governed-knowledge": true }
}
```

The ADR's reasons: "Explicit registration is the feature. It works identically for a knowledge base
sitting beside the project and one living anywhere else on disk, whereas inheritance-by-nesting works
only in the one layout that produced it." Its context is that Claude Code discovers `.claude/agents/`,
`.claude/skills/` and `.claude/commands/` by walking up from the working directory and stopping at the
repository root, so behavior placed in a workspace's `.claude/` is reachable from exactly one
directory. The ADR marks that discovery behavior "Documented, not verified here": it is "taken from
Claude Code's documentation" and "has not been observed in this environment".

All knowledge behavior travels in the one plugin. The ADR rejected an earlier split in which
maintenance behavior "stays home" and retrieval behavior "travels", because it "assumed a session would
always be started inside the knowledge base to curate it".

ADR-0006's status line reads "Accepted, amended 2026-09-19 (see *Amended 2026-09-19* below) and by
ADR-0007". ADR-0007 amends ADR-0006 and calls its inclusion of the facts in the same repository "the
problem": the engine now holds behavior and no domains (see [[design-thesis]], [[adr-0007-engine-content-separation]]).
The ADR-0006 table of what moved in names "The five maintenance skills"; the plugin at engine commit
5433f08 holds six skills in `knowledge-tools` and one in `analysis-tools`.

Source: [[adr-0006-behaviour-distribution]] · [[plugin-manifests]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'Explicit registration is the feature.' "$KB_ENGINE_ROOT/documents/decisions/0006-behaviour-distribution.md"

## Why the marketplace source stays a local directory, not a git source

ADR-0006's 2026-09-19 amendment asked whether the marketplace should now be sourced from the git
remote, and answered: "No. The `directory` source stays, and the reason is not inertia." A git source
"would give each consumer its own cached copy of the behaviour", while the facts are read from the
local clone, so behavior and facts would be "two independently-versioned things, and nothing keeps them
in step".

The ADR corrected its own first argument the same day. It had claimed behavior and facts "cannot
drift"; editing all five skills and then invoking one returned the previous version. Its finding:
"Plugin behaviour is loaded once at session start; facts are read live at query time." Its practical
consequence: "edit a skill, restart before trusting it." The lockstep argument "survives as a reason to
prefer a `directory` source over a `git` one (a git source adds a *second* axis of drift, across
revisions as well as across sessions), but it is a weaker claim than the one made here". The ADR
verified by session restart that the registration path resolves against the project root and that the
plugin's `installLocation` is the knowledge repository itself, "not a copy under
`~/.claude/plugins/cache/`".

A git source "becomes right for a consumer that holds **no local clone** and queries the knowledge base
some other way", and when it does "the drift problem has to be solved explicitly rather than
inherited". The ADR's review triggers are: "the relative marketplace path proves unsupported; a
consumer needs a pinned version of the behaviour rather than whatever the directory currently holds; or
a consumer appears that holds no local clone of the knowledge base".

The load-once-at-session-start fact is already covered: design-thesis.md § How the separation holds
depends on the runtime.

Source: [[adr-0006-behaviour-distribution]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'The `directory` source stays' "$KB_ENGINE_ROOT/documents/decisions/0006-behaviour-distribution.md"

## A project's own agent silently replaces a plugin agent of the same name

ADR-0006 names it "A hazard worth naming": project `.claude/agents/` shadow same-named plugin agents.
"That is a feature — a project can override a general retrieval agent with its own — and a
silent-divergence risk, because nothing announces that the general one has been replaced."

Source: [[adr-0006-behaviour-distribution]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'Project `.claude/agents/` **shadow** same-named plugin agents.' "$KB_ENGINE_ROOT/documents/decisions/0006-behaviour-distribution.md"

## The tier each file declares, and what a declaration does and does not restrict

Every skill, agent and command in the plugins declares a `tier:` in its frontmatter. The engine's
`check-tool-scope` lint holds the table, and its docstring says to "keep this table small — it is the
whole contract":

| Tier | Meaning in the lint | Tools it implies |
|---|---|---|
| `retrieval` | read-only research (kb-retrieve) | Read, Grep, Glob |
| `curation` | may write knowledge: the kb-* maintenance skills; Bash is for the engine's own check-* scripts and git, never an arbitrary command | Read, Grep, Glob, Write, Edit, Bash |
| `verification` | runs an assertion and reports it; the docstring says nothing in the behavior layer holds this tier | Bash |
| `analysis` | read-only retrieval plus Write, for the intelligence-analysis skill, whose procedure confines that Write to one user-level coaching log | Read, Grep, Glob, Write |
| `triage` | the reference desk (kb-ask): runs kb-catalog and starts kb-retrieve specialists; reads no library itself, so declares no Read | Bash, Agent |

Declared by file: `kb-ask` is `triage`; `kb-retrieve` is `retrieval`; `kb-update-domain`,
`kb-create-domain`, `kb-distill`, `kb-organize-domain`, `kb-audit`, the `kb-capture` command and the
`kb-init` command are `curation`; `intelligence-analysis` is `analysis`.

A subagent's `tools:` "is a real allowlist". A skill's or command's `allowed-tools:` "does NOT
restrict": it "only pre-approves the named tools for the turn that invokes it", so it is "a
declaration of intent, not a fence". The lint "checks that the declaration is present, well-formed, and
internally consistent with a stated tier". It "does NOT prove a skill or command is incapable of using
an undeclared tool". The lint's status as the least-privilege control is in [[agent-lifecycle-roles]]
(§ the mutation-governance controls).

Source: [[script-check-tool-scope]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'A SUBAGENT' "$KB_ENGINE_ROOT/scripts/check-tool-scope" && grep -qF 'is a real allowlist' "$KB_ENGINE_ROOT/scripts/check-tool-scope"

## kb-ask: the default way to answer from the knowledge base

**What it is for.** Answering a question from curated knowledge instead of from the model's own recall.
Its description: "The default way to answer from the knowledge base: use it whenever a question should
be answered from curated knowledge rather than from the model's own recall." It reads one compact
catalog of the registered libraries, sends the question to the one library specialist it belongs to
("a second only when needed, never more than two"), and passes back the specialist's cited answer
unchanged.

**Do not use it** "to curate, fix or add knowledge": that is the curation skills below.

**Tier and tools.** `triage`; `allowed-tools: Bash, Agent`. It runs the engine's `scripts/kb-catalog` and
starts `kb-retrieve` agents. It reads no library file itself.

**Gate.** None stated.

The desk's design, its triage cost against a single agent, and how it routes are in
[[retrieval-agent-behavior]] § A question goes to a triage desk, which sends it to one library's
specialist. Its own rules are that skill's behavior and are not restated here.

Source: [[skill-kb-ask]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'Do NOT use to curate, fix or add knowledge.' "$KB_ENGINE_ROOT/plugins/knowledge-tools/skills/kb-ask/SKILL.md"

## kb-retrieve: the library specialist agent

**What it is.** The one agent in the plugin. Its description: "The library specialist that the kb-ask
skill starts — load a library's INDEX, descend to the file(s) that answer, cite every one, and answer
only from what was read."

**When to use it.** "To answer a question from the knowledge base, use the kb-ask skill, which picks the
library and starts this agent. Call this agent directly only when the library is already known."

**Tier and tools.** `tier: retrieval`, `tools: Read, Grep, Glob`, `model: sonnet`. As a subagent its
`tools:` list is a real allowlist, so it cannot write.

**Gate.** None stated.

Its design and how it decides where to look are in [[retrieval-agent-behavior]]; the roles it plays are
in [[agent-lifecycle-roles]]. The rules inside the agent's prompt are not restated here.

Source: [[agent-kb-retrieve]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'Call this agent directly only when the library is already known.' "$KB_ENGINE_ROOT/plugins/knowledge-tools/agents/kb-retrieve.md"

## kb-update-domain: fold source material into a domain that already exists

**What it is for.** Folding "a spec, vendor page, legacy document, prior agent prompt, or system output"
into an existing knowledge domain, "with per-section provenance, dedup and supersede handling, and
contradictions flagged rather than resolved". The skill calls itself "The workhorse, and the most
dangerous skill in the set", because a language model folding a source "rounds a constant, drops a
qualifier, merges two behaviors into one tidy generalization".

**Use a neighbor instead.** "Do NOT use to create a domain (kb-create-domain), reshape a lumpy one
(kb-organize-domain), or distill session notes (kb-distill)." If the domain does not exist, the skill
stops and says `kb-create-domain` is the skill. A multi-domain source "folds once per target domain".

**Tier and tools.** `curation`; `allowed-tools: Read, Grep, Glob, Bash, Write, Edit`.

**Gate.** Step 8: "Present every extracted fact side by side with its source excerpt, and stop for
review." It states this "is not optional or summarizable". Only after that review does it commit. Its
checks run before the gate is presented.

**Refusals.** It stops and says so rather than proceeding, if:

- the engine root is unconfigured;
- the target library is marked `(read-only)` in your session context. It is a consumer's copy of
  someone else's library (ADR-0020): it is served here, never curated here. Say what you would have
  changed, and that it is filed with the library's publisher (`kb-libraries --read-only` names its
  origin). Never write to it, and never file the issue yourself;
- the target domain does not exist — that is `kb-create-domain`;
- the source material is not actually available to you. **Never fold from memory of what a
  document probably says.** A fold with no source is invention wearing a citation;
- you cannot preserve a value or qualifier faithfully;
- the only way to record a security finding is to include the secret's value or a person's
  identity;
- the material belongs in a different tier, or in a domain that does not exist yet;
- the user asks you to resolve a contradiction as part of the fold.

**Scripts it runs.** `scripts/check-kb <library-path>`, `scripts/check-scope <library-path>`,
`scripts/check-golden <library-path>` and `scripts/check-xlinks --installed`, before its first write and
again after. It states that a clean run means "structurally sound — not verified". A check that already
failed before it started is reported as pre-existing and left alone.

The skill's Steps 2 to 9 are its own text and are not restated here.

Source: [[skill-kb-update-domain]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF -- '- you cannot preserve a value or qualifier faithfully;' "$KB_ENGINE_ROOT/plugins/knowledge-tools/skills/kb-update-domain/SKILL.md"

## kb-create-domain: stand up a domain that does not exist yet

**What it is for.** "Stand up a new semantic knowledge domain that does not exist yet, in the correct
knowledge tier, conforming to the knowledge conventions — folder, INDEX router, frontmatter,
provenance, golden set, and the router chain above it." The skill states it is "Structure only — you are
building the shelf, and seeding it if source material was supplied. You are not inventing facts." A
domain is created as its own git repository, registered on the machine with `kb-library add`.

**Use a neighbor instead.** "Do NOT use to add facts to a domain that already exists (that is
kb-update-domain) or to reshape a lumpy one (kb-organize-domain)." When the contract and the skill
disagree, "the contract wins".

**Tier and tools.** `curation`; `allowed-tools: Read, Grep, Glob, Bash, Write` (no Edit).

**Gates.** Two. Step 1 asks about the tier rather than assuming it: "If you cannot tell, **ask the
user**." Step 2b requires the plan to be stated and approved "before Step 3 runs a single command":
the domain name and where it lives, the `scope:` and the tier test that produced it, and whether it
will be seeded. An unambiguous instruction from the user that covers all three means it proceeds without
re-asking. Step 7 then stops before committing: "do not run `git commit` inside the new library until the
human approves it." Where it seeds from source material, "Present each extracted fact to the user
against its source before committing."

**Refusals.** It stops and says so rather than proceeding, if:

- the engine root is unconfigured — never guess a path;
- the domain already exists — that is `kb-update-domain`;
- a library of that name is already registered on this machine (`kb-library list`);
- the tier is genuinely ambiguous and the user has not decided;
- you would have to invent a fact to fill the domain. An empty, well-formed domain is a good
  outcome. A domain full of plausible guesses is contamination of the one place facts are
  supposed to be trustworthy.

**Scripts it runs.** `kb-library home`, `kb-library list` and `kb-library add <dir>/<domain>`, and
afterwards `scripts/check-kb <library-path>`, `scripts/check-golden <library-path>` and
`scripts/check-scope <library-path>`. It also runs `git init` in the new library and, once the library has
a GitHub remote, `gh label create owner-approved`. Of that label it says: "**Create it; never apply
it.**"

With no content yet, the golden set is "deferred and no file is created"; `kb-update-domain` seeds it
"when the domain gains its first facts, in the same change".

Source: [[skill-kb-create-domain]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF -- '- a library of that name is already registered on this machine (`kb-library list`);' "$KB_ENGINE_ROOT/plugins/knowledge-tools/skills/kb-create-domain/SKILL.md"

## kb-distill: turn episodic notes into durable facts

**What it is for.** Turning "episodic material — session logs, meeting notes, debugging transcripts,
dump records — into durable semantic facts, separating what was observed from what was concluded or
merely speculated, and citing the episodic note as the source." The skill's point is that "a session log
does not mark the difference" between an observation and a guess.

**Use a neighbor instead.** "Do NOT use to fold external documents (kb-update-domain), create a domain
(kb-create-domain), or delete the episodic record." The note stays: "Distilling does not consume the
note", and deletion "is never automated and is not this skill's business". To record a session as a note
in the first place, use the `/kb-capture` command below.

**Tier and tools.** `curation`; `allowed-tools: Read, Grep, Glob, Bash, Write, Edit`.

**Gate.** Step 7: it presents every statement it classified, "including the ones you refused to
promote, and why", every fact promoted against its note line, and every scope decision, then "**Stop here
for review. Only after the human approves do you commit.**" The facts are "already written into the
library's working tree, not the library's history". In prepare mode it cannot write, and the person
decides from the review packet later; that split is in [[unattended-preparation]].

**Refusals.** It stops rather than proceeding, if:

- the engine root is unconfigured;
- the target library is marked `(read-only)` in your session context. It is a consumer's copy of
  someone else's library (ADR-0020): it is served here, never curated here. Say what you would have
  changed, and that it is filed with the library's publisher (`kb-libraries --read-only` names its
  origin). Never write to it, and never file the issue yourself;
- the episodic material is not actually available to you — never distil from memory of a session;
- no domain exists for the facts, and you would have to invent one on the fly;
- the only way to record a security finding is to include the secret's value or a person's
  identity — record the reference, or leave the finding in the episodic note;
- you would have to promote a speculation, a decision, or a single observation into a general
  truth to make the distillation look productive. **An episodic note that yields two facts and
  eight refusals is a good outcome.**

**Scripts it runs.** `scripts/check-kb <library-path>`, `scripts/check-golden <library-path>` and
`scripts/check-transient <library-path>`; for a review packet it also uses `kb-apply list`, `kb-apply show
<packet>` and `kb-apply done <packet> applied` (or `rejected`).

Already covered, not restated: what counts as a fact and the current-state rule for fact files in
[[agent-lifecycle-roles]] § a fact file holds current state; the credential rule in [[secret-governance]];
prepare mode and `kb-apply` in [[unattended-preparation]].

Source: [[skill-kb-distill]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'eight refusals is a good outcome' "$KB_ENGINE_ROOT/plugins/knowledge-tools/skills/kb-distill/SKILL.md"

## kb-organize-domain: reshape a lumpy domain, structure only

**What it is for.** "Reshape a lumpy knowledge domain — split an overgrown file, merge over-fragmented
ones, introduce a sub-index when a domain exceeds the flat threshold, and keep every router, backlink and
golden-set record in step. Structure only, never meaning." The skill "Moves knowledge; never changes it".
Its Step 1 names the valid reasons to reshape, including a domain over "roughly **15** topic files". If
none holds it will "say the domain is fine and stop": "A domain that is merely large is not lumpy."

**Use a neighbor instead.** "Do NOT use to add facts (kb-update-domain), create a domain
(kb-create-domain), or resolve contradictions."

**Tier and tools.** `curation`; `allowed-tools: Read, Grep, Glob, Bash, Write, Edit`.

**Gate.** Step 7: "**Stop here for review. Only after the human approves do you commit.**" Every move,
split, merge and rename "already happened in the working tree, not in the library's history". On a
rename it says "ask rather than coin one" when two names are genuinely equal. A rename is "the expensive
move", paid for in the same change: the file's `name:`, every `[[slug]]` in this and every other installed
library, every router line and every golden-set `expected_file:`.

**Refusals.** It stops rather than proceeding, if:

- the engine root is unconfigured;
- the target library is marked `(read-only)` in your session context. It is a consumer's copy of
  someone else's library (ADR-0020): it is served here, never curated here. Say what you would have
  changed, and that it is filed with the library's publisher (`kb-libraries --read-only` names its
  origin). Never write to it, and never file the issue yourself;
- the domain does not exist, or holds no content to organize;
- you would have to change a fact's meaning to make the structure nicer. **The structure loses**;
- the reshape would resolve a contradiction, drop a `Source:` link, or delete knowledge. None of
  those are this skill's business, and deletion is never automated;
- the domain is simply fine. Say so.

It leaves the router's `freshness_horizon` and `verifier_budget` alone: "Those are currency policy, not
structure".

**Scripts it runs.** `scripts/check-kb <library-path>`, `scripts/check-golden <library-path>`,
`scripts/check-xlinks --installed` and `scripts/check-scope <library-path>`, before its first change as
well as after; and `kb-library list` to find the other libraries to search for the old slug. It is fully
attended: it has no prepare mode ([[unattended-preparation]]).

Source: [[skill-kb-organize-domain]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'you would have to change a fact' "$KB_ENGINE_ROOT/plugins/knowledge-tools/skills/kb-organize-domain/SKILL.md"

## kb-audit: sweep for decay and queue findings, never fix them

**What it is for.** "Sweep a knowledge base for decay — dead links, orphaned files, stale or missing
currency stamps, index bloat, empty routed leaves, ageing contradictions and scope misfiling — and build
a prioritised needs-attention queue. Detection only; it proposes and never mutates." Use it "as the
acceptance gate after a wave of knowledge work, or as a recurring hygiene sweep". Its one discipline:
"it detects and it reports. It does not fix."

**Use a neighbor instead.** "Do NOT use it to fix what it finds."

**Tier and tools.** `curation`; `allowed-tools: Read, Grep, Glob, Bash, Write, Edit`. Its Step 4 calls the
`last_swept` line "the sweep's second and last mutation": the first is the queue file
`<library>/needs-attention.md`, written only if there are findings.

**Gate.** A person decides in the resume half. In prepare mode the skill is run headless, "with only
Read, Grep and Glob", and "your reply **is** the review packet"; a person later reviews it, "this is the
decide half, and it is always attended". On resume it re-runs Step 1's checks, presents "the records one
decision each: approve, edit or reject", and "Write nothing before the person has decided". An audit
"always produces a packet, even a clean one", because applying it records `last_swept`. The packet split
is in [[unattended-preparation]]; the queue and its use are in [[needs-attention]].

**Refusals.**

- **Do not fix what you find.** Not the dead link, not the missing stamp, not the obvious typo in a
  router line. Every one of those is a mutation, and mutation is gated behind a human.
- Do not resolve a contradiction, delete a file, or re-stamp a `Verified:` date. Re-stamping in
  particular is manufacturing freshness — you did not re-verify anything, you ran a sweep.
- Do not report a clean sweep as a verified knowledge base. State the coverage gap every time.
- **Do not sweep a library marked `(read-only)`** in your session context. It is a consumer's copy of
  someone else's library (ADR-0020): its queue and its `last_swept` belong to its publisher. Say so,
  and name its origin (`kb-libraries --read-only`) for anything the person wants to file there.

It also refuses to invent a category: "Something real that fits none of these categories goes in your
report, not the queue."

**Scripts it runs.** Step 1: `scripts/check-kb <library-path>`, `scripts/check-scope <library-path>`,
`scripts/check-golden <library-path>`, `scripts/check-xlinks --installed` and
`scripts/check-transient <library-path>`. For each queue record's owner it runs `scripts/kb-owner <tree>
<file>`, "never by reading CODEOWNERS yourself". On resume it uses `kb-apply list`, `kb-apply show
<packet>` and `kb-apply done <packet> applied` (or `rejected`). `kb-prepare audit <library>` is what runs
it headless.

The severity order, the record shape, the per-owner cap (default 10 per sweep) and the 14-day
escalation are the skill's own text. What a clean sweep proves is in [[guardrail-verification]].

Source: [[skill-kb-audit]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF -- '- Do not report a clean sweep as a verified knowledge base. State the coverage gap every time.' "$KB_ENGINE_ROOT/plugins/knowledge-tools/skills/kb-audit/SKILL.md"

## /kb-capture: record what the session learned as an episodic note

**What it is for.** The command "Capture what this session learned as an episodic note in the nearest
knowledge tree. Records; does not classify." Argument hint: `[title] [--into TREE]`. It writes the body
of the note; "Filing is the script's." The one rule: "**Keep what was observed separate from what was
believed, and never upgrade one into the other.**" It writes two sections, `## What happened` and
`## What we believe but did not check`, and the title describes "what was **done and seen**, never what
was concluded".

**Use a neighbor instead.** It does not "decide what is a fact, what tier it is, or which domain it
belongs to — that is distillation" (`kb-distill`). It adds no currency stamp or `Source:` line.

**Tier and tools.** `curation`; `allowed-tools: Bash, Write`.

**Gate.** None stated for a person: the command files the note by running the script. It stops and says
so where the script reports there is no tree: "do not create one yourself, and do not file the note
somewhere improvised".

**Refusals.**

- If `KB_ENGINE_ROOT` is unset, it says so and stops.
- Free text typed after `/kb-capture` is "a hint for the title, not an argument": "Never pass free text
  to the script as a positional argument", because the script's positional is a directory.
- If the script refuses a library registered read-only (ADR-0020), it relays that, and offers "the
  project's own tree, or a note for the person to file with the library's publisher".
- The script itself refuses a note holding a credential or a personal identifier; that boundary is in
  [[secret-governance]].

**Scripts it runs.** `"$KB_ENGINE_ROOT/scripts/kb-capture" --title "<what was done and seen>" --from
<tmpfile> [--into <tree>]`.

Source: [[command-kb-capture]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'Records; does not classify.' "$KB_ENGINE_ROOT/plugins/knowledge-tools/commands/kb-capture.md"

## /kb-init: scaffold a project knowledge tree

**What it is for.** The command's description: "Scaffold a project knowledge tree — knowledge/INDEX.md,
plus .claude/ and CLAUDE.md only if absent. Non-destructive and safe to re-run." Argument hint: `[--name
NAME] [DIR]`. Its body says "Run the engine's `kb-init` script and report its output verbatim", and: "That
is the whole command." The logic lives in the script "because scaffolding a project tree involves no
judgment and therefore needs no model — and because a developer must be able to run the identical thing
from a terminal with no Claude session at all."

**Tier and tools.** `curation`; `allowed-tools: Bash`.

**Gate.** None stated: it runs the script and reports its output.

**Refusals.** "If `KB_ENGINE_ROOT` is unset, say so and stop. Do not reconstruct the scaffold by hand: an
agent improvising the steps is exactly what the script exists to replace."

**Scripts it runs.** `"$KB_ENGINE_ROOT/scripts/kb-init" $ARGUMENTS`. Pass `DIR` when the target is not the
current project: "the script resolves the enclosing git repository from wherever it is pointed".

Source: [[command-kb-init]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'That is the whole command.' "$KB_ENGINE_ROOT/plugins/knowledge-tools/commands/kb-init.md"

## intelligence-analysis: review an analytic product, or coach the user

**What it is for.** Reviewing a work product for analytic rigor, or coaching the user in
intelligence-analysis tradecraft. It applies "structured analytic techniques, a bias-mitigation catalog,
and an analytic-standards rubric" to "*any* reasoned product". Review mode says where the thinking is
strong and weak, "anchored to specific passages, with a concrete fix for each weakness"; coach mode
teaches "one technique at a time against their own real work". It "holds no tradecraft — it retrieves
it" from the `intelligence-analysis` knowledge library at run time.

**Use a neighbor instead.** "Do NOT use for: cleaning up prose or removing AI-writing tells (that's the
`humanizer` skill); reviewing code for correctness, bugs, or style (that's `code-review`); or writing a
user story from scratch (that's `create-user-story` — though this skill pairs well as a review pass over
a finished story)." It is for the quality of thinking "rather than its prose or its code".

**Tier and tools.** `analysis` (in the `analysis-tools` plugin); `allowed-tools: Read, Grep, Glob, Write`.
The Write is for the coaching log only (see below).

**Gate.** No stop for a person is stated for review mode. Coach mode has a standing one: "When you pose a question meant
to make the user think, **that question ends your turn.**" It emits nothing after it and waits for the
person. A genuinely ambiguous request gets "**one** clarifying question, then proceed".

**What it does in review, in brief.** It first places the product on a ladder of Light, Medium or Full
and holds it to that bar only. Each level has a finding cap: Light "0–1 finding", Medium "~2–3
findings", Full "~3–5 findings". The caps are "ceilings on a *prioritized* list, not quotas", and a
genuinely clean product "gets **zero** findings and a plain 'this is sound.'" The levels and caps are
the skill's own behavior; the skill says they are not in the library. Its output always ends with a
`Sources:` line naming every library file drawn on.

**Refusals.**

- If the knowledge library is not installed it says so, "the intelligence-analysis knowledge library
  isn't installed, so I can't run a grounded review or coaching session", and stops: "Do not fall back to
  reviewing from memory."
- "Never read or search a file named `*-golden.md`, or anything under a `documents/eval-*/` folder."
- "Refuse a `CONFLICTED` section": name both claims and who holds them, and "do not pick one, average
  them, or hedge around the conflict".
- "Say when a fact isn't there": `fact not found in the intelligence-analysis library`, and proceed with
  what it has, naming the gap.
- Its rating scale (Excellent, Good, Fair, Poor, N/A) is its own, and it must never be presented as the
  IC's own: ICD 203 "refers to evaluation rating criteria it does not publish".

**Scripts it runs.** None: it declares no Bash. It reads the library its session context lists and writes
one file, `~/.claude/ia-coaching-log.md`, "a user-level file outside every repository". "Only a coaching
session creates it. A review never creates the file"; the log is "not a knowledge library", carries no
router entry, and is not subject to the retrieval contract.

Source: [[skill-intelligence-analysis]]
Verified: 2026-11-21 · by: agent · method: doc-review
Recheck: grep -qF 'and stop. Do not fall back to' "$KB_ENGINE_ROOT/plugins/analysis-tools/skills/intelligence-analysis/SKILL.md"
