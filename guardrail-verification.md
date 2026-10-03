---
name: guardrail-verification
description: Why a guardrail that is correct on the machine that wrote it can still fail on every other machine or catch nothing at all — defects found only by running or by a fresh reader, a bug that recurs, CI coverage, filename-pattern guards, regression-suite blind spots, a scanner that exits 0 after printing findings, and the difference between a clean sweep and a true fact.
memory_type: semantic
domain: knowledge-architecture
scope: general
verified: 2026-10-02
metadata:
  type: fact
  node_type: memory
  created: 2026-09-22
tags: [knowledge-architecture, guardrails, ci, testing]
keywords: [check-exec-bits, check-kb, regression suite, core.fileMode, PyYAML, kb-audit, kb-verify, blind spot, negative control, CI coverage, inspection, fresh reader, recurring defect, subshell counter, exit status, check-secrets]
---
# Guardrail verification

A guardrail passing proves only that it found no defect of the kind it looks for. Two defects
reached the remote in one day, both invisible on the machine that wrote them, and both expose a
different way a guardrail can pass while the thing it guards is broken.

## Almost nothing important was found by reading

Building this architecture found its defects by running something and checking the result against what it claimed, almost never by inspection. Code that read correctly exited with the wrong status, a contract that read consistently contradicted itself, and skills that read as complete were underspecified in ways a fresh reader hit at once. The single highest-value technique was handing an artifact to someone who had not written it and asking what was ambiguous.

The same holds for documentation. A one-page contributor guide was tested by giving it, and nothing else, to a fresh agent, which recorded a realistic session correctly and whose feedback closed three real gaps in the page. The guide's own redaction example, a connection URL with the password starred out, was rejected by the engine's secret scan because that shape matches a real credential. A guide is proved by a reader, not by a word count.

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§9.1, §9.19) · [[adr-0013-steady-state-surfaced-not-scheduled]] · ai-kb@32112f2
Verified: 2026-10-02 · by: agent · method: doc-review

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

Source: [[2026-09-22-two-guardrail-defects-reached-the-remote-scripts-committed]] · ai-kb@21758c6
Verified: 2026-10-02 · by: agent · method: manual
Recheck: t=$(mktemp -d); git init -q "$t/r"; git -C "$t/r" config core.fileMode false; printf "#!/bin/sh\necho hi\n" > "$t/r/s"; chmod +x "$t/r/s"; git -C "$t/r" add -A; git -C "$t/r" -c user.email=t@t -c user.name=t commit -qm add; mode=$(git -C "$t/r" ls-files -s -- s | cut -d" " -f1); rm -rf "$t"; [ "$mode" = "100644" ]

## A defect that recurs needs a check that detects it, not another fix

The non-executable-script defect was found, fixed and written into a decision record, and then occurred twice more: once when new hook files were created during a restructure, once at library creation. The fix that held was not another fix but a check, `check-exec-bits`, run in every hook and in CI. That held for the files it knew how to find. It missed a fourth occurrence, described above, because its rule matched paths rather than what a file is. The commit that fixed the fourth occurrence says cloning into a throwaway directory and running the result as a teammate would is "the only way this class of bug is ever found".

Source: [[knowledge-agent-architecture-combination-plan-revised]] (§9.6) · [[2026-09-22-two-guardrail-defects-reached-the-remote-scripts-committed]] · ai-kb@21758c6
Verified: 2026-10-02 · by: agent · method: doc-review

## A check nobody's CI executes guards nothing, even if it is correct on every developer's machine

`check-kb` imported PyYAML from the day it was written. Nobody noticed, because at the time the
engine's own CI ran only exec-bit, secret and embedded-fact checks — the engine holds no knowledge for
`check-kb` to read, so the engine's CI had never executed it — and every developer machine so far
happened to have the module installed already. The first CI run that actually exercised it, once a
regression suite was added, failed six times with the same error:

    ModuleNotFoundError: No module named 'yaml'

The fix removed the dependency rather than installing it in three workflows: every script in
`scripts/` now parses frontmatter with the standard library only.

The engine's CI has grown since: its workflow now also runs tool-scope, stamp-honesty and
high-blast-radius steps and the regression suite, as read from the workflow file on 2026-10-02. The
reason `check-kb` was never among them is unchanged, since the engine still holds no knowledge for it to read.

Source: [[2026-09-22-two-guardrail-defects-reached-the-remote-scripts-committed]] · ai-kb@13d81cf · ai-kb@3050ef8
Verified: 2026-10-02 · by: agent · method: doc-review

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

Source: [[2026-09-22-two-guardrail-defects-reached-the-remote-scripts-committed]] · ai-kb@3050ef8
Verified: 2026-10-02 · by: agent · method: doc-review

## A clean kb-audit sweep proves structural soundness, not that a fact's content is still true

`kb-audit` ran over all three trees on the same day and reported the knowledge structurally sound,
which, in structural terms, it was. A fact elsewhere in the same fold still quoted a hook body that
had been rewritten two commits earlier, carrying a `Verified:` stamp only a day old — recent enough
to look current. A sweep checks that stamps exist and are not stale by date; it does not check a
fact's content against the live system that fact describes. The defect was found by reading the hook,
not the knowledge base. The plan's account of why every other check missed it is that both the structural
checks and a careful sweep "were reading the knowledge base rather than the thing it describes". The
`Recheck:` assertion and the Verifier are the mechanism built to check content. Whether one would have
caught that fact sooner is untested: the assertion that catches it today was written after the fact was
already wrong (see [[knowledge-architecture-open-questions]]).

Source: [[2026-09-22-two-guardrail-defects-reached-the-remote-scripts-committed]] · [[knowledge-agent-architecture-combination-plan-revised]] (§9.13)
Verified: 2026-10-02 · by: agent · method: doc-review

## A scanner that prints its findings and exits 0 is worse than none

A secret scanner printed every finding and exited 0. It piped its output into a function, so the counter it incremented lived in a subshell and was discarded. It warned and then waved the commit through, which is worse than no guardrail because it reads as protection. The header of `check-secrets` now carries the reason in a comment: findings are captured through command substitution, "never by piping into a function", because "a pipeline component runs in a subshell, so a counter incremented there is discarded and the script would print findings while exiting 0". Like the other defects the plan lists, it was found by running the scanner and checking its exit status against what it had printed.

Source: [[script-check-secrets]] · [[knowledge-agent-architecture-combination-plan-revised]] (§9.6)
Verified: 2026-10-02 · by: agent · method: doc-review
