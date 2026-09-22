---
name: 2026-09-22-two-guardrail-defects-reached-the-remote-scripts-committed
description: "Two guardrail defects reached the remote: scripts committed 644 and check-kb importing PyYAML; both invisible on the authoring machine"
memory_type: episodic
domain: knowledge-architecture
scope: general
metadata:
  type: episodic
  node_type: memory
  created: 2026-09-22
tags: [knowledge-architecture, episodic]
keywords: [episodic, session]
---
# 2026-09-22 — Two guardrail defects reached the remote: scripts committed 644 and check-kb importing PyYAML; both invisible on the authoring machine

## What happened

Two guardrail defects reached the remote today. Both were invisible on the machine that wrote them.

**1. Scripts recorded non-executable.** `kb-bootstrap` and `kb-session-start` were committed mode
`100644`. On the authoring machine they ran: the on-disk bit is set, and `core.fileMode` is false on
this WSL mount, so git records 644 without complaint. Found by cloning the engine into a throwaway
directory and running it as a teammate would:

    env: '.../engine2/scripts/kb-bootstrap': Permission denied

`check-exec-bits` reported clean throughout. Its rule matched `.githooks/*`, `*/hooks/*` and
`scripts/check-*` — the two new scripts matched none of those patterns. The check exists because
this same defect had already recurred three times.

**2. `check-kb` imported PyYAML.** Committed at the start and never noticed. The engine's CI runs
only exec-bits, secrets and embedded-facts, because the engine holds no knowledge for `check-kb` to
read, so CI had never executed it. Every developer machine so far had the module. The first CI run
of the new regression suite produced, six times:

    ModuleNotFoundError: No module named 'yaml'

Every other script in `scripts/` parses frontmatter with the standard library. The fix was to remove
the dependency rather than install it in three workflows.

**3. The regression suite itself had a blind spot, and then a bug.** Written to catch exactly this
class, it was proved by reintroducing two real past bugs. It caught the `check-exec-bits` pattern
regression. It did **not** catch the `check-xlinks` episodic-exclusion regression, because it had no
case citing a session note — the same blind spot the original bug lived in. Separately, a case
expecting only an exit status could never pass: `grep` finds nothing in empty output, so the
substring check failed on an empty expectation.

**4. The sweeps did not find either defect.** `kb-audit` ran over all three trees today and reported
the knowledge structurally sound, which it was. A fact in `test_project` describing the pre-commit
hook still quoted a hook body that had been rewritten two commits earlier; the stamp was one day
old. The defect was found by reading the hook, not the knowledge base.

## What we believe but did not check

- Other scripts probably have environment assumptions nobody has hit yet — GNU `sed -i`, `readlink
  -f` and `mktemp -d` are all used and all behave differently on macOS. No macOS machine has run any
  of this.
- Adding a `Recheck:` assertion to more facts would probably have caught the stale hook fact sooner,
  but the one that catches it today was written after the fact was already wrong, so that is a
  hypothesis about the future rather than an observation.

## Distilled

Distilled 2026-09-22 into [[guardrail-verification]] (all four sections: the `core.fileMode`/
filename-pattern defect, the CI-never-executed `check-kb` defect, the regression suite's own blind
spot and its grep/empty-output bug, and the kb-audit-is-structural-not-content-true observation).
The two "what we believe but did not check" items were left here as speculation, not promoted.
