# knowledge-architecture

A domain library for the [ai-kb](https://github.com/dhtaylor/ai-kb) engine. It holds what is
known about how a governed knowledge base and the thin agent layer over it are designed to work:
why knowledge is kept apart from behavior, the roles that keep facts current, and why the
guardrails around it can fail quietly.

It holds **facts about the design, not the design's rules**. The operative contract (scope tiers,
provenance, currency, contradictions, the retrieval contract) lives in the engine's
[`CONVENTIONS.md`](https://github.com/dhtaylor/ai-kb/blob/main/CONVENTIONS.md) and is not
duplicated here.

## What is in it

| File | What it covers |
|---|---|
| [`design-thesis.md`](../design-thesis.md) | Why knowledge and behavior are separated, the knowledge-as-behavior anti-pattern, and the target layering |
| [`agent-lifecycle-roles.md`](../agent-lifecycle-roles.md) | The Curator, Research-librarian and Fact-checker roles with the Verifier and Watcher as built, how changes to the KB are governed (built, dormant and not-built controls), and why no orchestrator is built and how retrieval cost is bounded |
| [`secret-governance.md`](../secret-governance.md) | Why rotating a leaked secret is not removing it, why a pre-commit scan is not the enforcement boundary, what the three-pass secret and identifier scan covers and cannot see, why capture refuses a note holding a credential, and the blind trap test of the distill gate |
| [`guardrail-verification.md`](../guardrail-verification.md) | Why a guardrail that is correct on the machine that wrote it can still fail everywhere else, or catch nothing at all |

Alongside the facts:

- [`INDEX.md`](../INDEX.md) is the router. Retrieval starts here, and anything it doesn't link to
  can't be reached.
- [`sources/`](../sources/INDEX.md) holds citation stubs for the artifacts the facts were folded
  from.
- [`episodic/`](../episodic/INDEX.md) holds session notes, newest first: the raw record that facts
  are distilled from.
- `documents/` holds the archived evidence itself.
- `knowledge-architecture-golden.md` is the golden set: the test questions retrieval is graded
  against.

`documents/` and the golden set are deliberately **not** routed from `INDEX.md`. They are
provenance and test apparatus, not knowledge, and an agent that could reach the golden set could
read the answers.

Every fact section carries a `Source:` backlink and a `Verified:` stamp saying when, by whom and
how it was last checked.

## Installing it

A library lives inside an engine, in a folder named after its **domain**, which here is not the
repository's name:

```bash
git clone https://github.com/dhtaylor/ai-kb-architecture.git <engine>/kb/knowledge-architecture
<engine>/scripts/kb-bootstrap --apply
```

`kb-bootstrap` registers the engine on the machine and activates this library's commit hook. Run
it again after cloning any library, because git does not clone hook configuration. The engine's
[README](https://github.com/dhtaylor/ai-kb#getting-started) covers setting up the engine itself.

To read from the library, ask a question through the engine's `kb-retrieve` agent. It descends
from `INDEX.md`, cites every file it reads, and says so when the library has no answer.

## Contributing

Add or change facts with the engine's skills, not by hand: `kb-update-domain` to fold in source
material, and `kb-distill` to turn episodic notes into facts. Both stop for human review before
anything is committed. The engine's
[`CONTRIBUTING.md`](https://github.com/dhtaylor/ai-kb/blob/main/CONTRIBUTING.md) is the working
guide.

The checks live in the engine; this repository does not carry its own copies.

- **Pre-commit hook** (`.githooks/pre-commit`). Runs the structure, scope, golden-set, secret and
  executable-bit checks. It also checks cross-library links when sibling libraries are installed
  beside it. If no engine is found, it warns and skips rather than failing.
- **CI** (`.github/workflows/guardrails.yml`). Clones the public engine anonymously, with no
  credentials, and runs the same checks, plus a full-history secret scan and a high-blast-radius
  scan.
- **The `owner-approved` label.** The high-blast-radius scan fails a pull request that deletes or
  renames away a fact file, removes a `CONFLICTED` marker or promotes a fact's scope, unless the
  pull request carries this label. `main` is protected with `guardrails` as a required check, so
  that failure blocks the merge. An admin can still push directly, but CI then only warns, and
  the bypass shows up in the history.
- **`CODEOWNERS`.** Names who owns each path, which is who a `kb-audit` finding is assigned to.
  It is advisory until branch protection makes owner review required.

**This repository is public.** Never commit a credential or a personal identifier. A security
finding records the secret's reference, its location and the action required, never the value
(`CONVENTIONS.md` §6). The secret scan blocks what it can recognise; the rest is on the author.
