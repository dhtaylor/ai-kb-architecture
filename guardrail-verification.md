---
name: guardrail-verification
description: Why a guardrail that is correct on the machine that wrote it can still fail on every other machine or catch nothing at all — CI coverage, filename-pattern guards, regression-suite blind spots, and the difference between a clean sweep and a true fact.
memory_type: semantic
domain: knowledge-architecture
scope: general
verified: 2026-09-22
metadata:
  type: fact
  node_type: memory
  created: 2026-09-22
tags: [knowledge-architecture, guardrails, ci, testing]
keywords: [check-exec-bits, check-kb, regression suite, core.fileMode, PyYAML, kb-audit, kb-verify, blind spot, negative control, CI coverage]
---
# Guardrail verification

A guardrail passing proves only that it found no defect of the kind it looks for. Two defects
reached the remote in one day, both invisible on the machine that wrote them, and both expose a
different way a guardrail can pass while the thing it guards is broken.

## Environment differences hide a defect from the authoring machine; guard by structure, not by filename

Two new scripts, `kb-bootstrap` and `kb-session-start`, were committed git mode `100644`
(non-executable). They ran without any problem on the authoring machine, because the on-disk
executable bit was set there and `core.fileMode` is false on that WSL mount, so git recorded the
committed mode without complaint. Only cloning into a fresh directory and running the scripts as a
teammate would surfaced the failure, with no prior warning:

    env: '.../engine2/scripts/kb-bootstrap': Permission denied

The guardrail meant to catch this, `check-exec-bits`, did not: its rule matched the paths
`.githooks/*`, `*/hooks/*` and `scripts/check-*`, and the two new scripts matched none of those
patterns, so the check reported clean. It was rewritten to test what a file *is* — whether it
starts with a shebang — rather than what it is named or where it lives, so a future script needs no
naming convention to be covered.

Source: [[2026-09-22-two-guardrail-defects-reached-the-remote-scripts-committed]]
Verified: 2026-09-22 · by: agent · method: manual
Recheck: t=$(mktemp -d); git init -q "$t/r"; git -C "$t/r" config core.fileMode false; printf "#!/bin/sh\necho hi\n" > "$t/r/s"; chmod +x "$t/r/s"; git -C "$t/r" add -A; git -C "$t/r" -c user.email=t@t -c user.name=t commit -qm add; mode=$(git -C "$t/r" ls-files -s -- s | cut -d" " -f1); rm -rf "$t"; [ "$mode" = "100644" ]

## A check nobody's CI executes guards nothing, even if it is correct on every developer's machine

`check-kb` imported PyYAML from the day it was written. Nobody noticed, because the engine's own
CI runs only exec-bit, secret and embedded-fact checks — the engine holds no knowledge for
`check-kb` to read, so the engine's CI had never executed it — and every developer machine so far
happened to have the module installed already. The first CI run that actually exercised it, once a
regression suite was added, failed six times with the same error:

    ModuleNotFoundError: No module named 'yaml'

The fix removed the dependency rather than installing it in three workflows: every script in
`scripts/` now parses frontmatter with the standard library only.

Source: [[2026-09-22-two-guardrail-defects-reached-the-remote-scripts-committed]]
Verified: 2026-09-22 · by: agent · method: manual

## A regression suite inherits the blind spot of the bug it was built to catch, one case at a time

The regression suite was written to catch exactly this class of defect, and was proved by
reintroducing two real past bugs. It caught the `check-exec-bits` pattern regression. It did not
catch a `check-xlinks` regression that excluded episodic notes, because the suite had no case citing
a session note — the same shape of input the original bug lived in. A suite only catches the shapes
of input it has a case for; reintroducing the bug it was named for is not the same as reintroducing
every bug it was meant to cover.

Separately, a case in the same suite expected only an exit status, with no expected substring — and
could never pass, because the harness checks output by grepping for a substring, and grep finds
nothing in empty output. The check failed even when the thing under test was behaving correctly.

Source: [[2026-09-22-two-guardrail-defects-reached-the-remote-scripts-committed]]
Verified: 2026-09-22 · by: agent · method: manual

## A clean kb-audit sweep proves structural soundness, not that a fact's content is still true

`kb-audit` ran over all three trees on the same day and reported the knowledge structurally sound,
which, in structural terms, it was. A fact elsewhere in the same fold still quoted a hook body that
had been rewritten two commits earlier, carrying a `Verified:` stamp only a day old — recent enough
to look current. A sweep checks that stamps exist and are not stale by date; it does not check a
fact's content against the live system that fact describes. Only a `Recheck:` assertion or a
Verifier pass catches a fact that was re-stamped without actually being re-checked.

Source: [[2026-09-22-two-guardrail-defects-reached-the-remote-scripts-committed]]
Verified: 2026-09-22 · by: agent · method: manual
