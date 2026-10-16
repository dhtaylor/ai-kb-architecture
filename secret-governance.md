---
name: secret-governance
description: Secret and personal-identifier governance — why rotation is not removal, why a pre-commit scan is not the enforcement boundary, what the engine's three-pass scan covers and cannot see, why raw artifacts archived in a library skip the bare-entropy rule and UTF-16 text is blocked, why capture refuses a note holding a credential, and the evidence a gate works.
memory_type: semantic
domain: knowledge-architecture
scope: general
verified: 2026-10-12
metadata:
  type: fact
  node_type: memory
  created: 2026-09-19
tags: [knowledge-architecture, secrets, governance]
keywords: [secret scan, pre-commit, archived artifact, schema dump, bare rule, UTF-16, UNSCANNED, rotation, history rewrite, git filter-repo, push protection, server-side gate, check-secrets, entropy, PII, personal identifier, allowlist, lint-allow, kb-capture, working tree, reference not value, blind trap test]
---
# Secret governance

The architecture keeps secrets out of both KB trees by reference (vault or env, never inlined),
guarded by a secret-scanner pre-commit hook over both trees. These properties of that guard are
easy to get wrong:

## Rotation is not removal

Rotating a leaked secret does not erase it from git *history* — every clone still holds it. Any
existing leak requires **history rewrite** (e.g. `git filter-repo --invert-paths`), force-push,
clone invalidation, and a clean full-log scan.

Source: [[knowledge-agent-architecture-combination-plan]] (§5.4)
Verified: 2026-10-12 · by: agent · method: manual
Recheck: t=$(mktemp -d); git init -q "$t/r"; printf "SECRET=abc123\n" > "$t/r/f"; git -C "$t/r" add -A; git -C "$t/r" -c user.email=t@t -c user.name=t commit -qm add; rm "$t/r/f"; git -C "$t/r" add -A; git -C "$t/r" -c user.email=t@t -c user.name=t commit -qm remove; git -C "$t/r" log -p --all | grep -q "SECRET=abc123"; r=$?; rm -rf "$t"; exit $r

The engine's `check-secrets` says the same in its closing advice after a scan of git finds a hit, in these words: "A real leak needs rotation AND history rewrite — rotation alone does not erase it from git history; every existing clone still holds it." That advice is printed for the staged, worktree and full-history scans; for a scan of one file that has never been committed it prints "If this is a live credential, rotate it at the source — it is live wherever it was copied from, whether or not this file is ever committed" (see the capture boundary below).

Source: [[script-check-secrets]]
Verified: 2026-10-12 · by: agent · method: doc-review
Recheck: s=$(cd ../../scripts && pwd) && t=$(mktemp -d) && r=0; export GIT_CONFIG_GLOBAL=/dev/null GIT_CONFIG_NOSYSTEM=1; git init -q "$t/r"; p=Ab1#; printf 'password = %s%s%s\n' "$p" "$p" "$p" > "$t/r/f"; git -C "$t/r" add f; (cd "$t/r" && "$s/check-secrets" >"$t/staged.out" 2>&1); [ $? -eq 1 ] || r=1; "$s/check-secrets" --file "$t/r/f" >"$t/file.out" 2>&1; [ $? -eq 1 ] || r=1; grep -q 'rotation alone does not' "$t/staged.out" && grep -q 'history rewrite' "$t/staged.out" || r=1; grep -q 'rotate it at the source' "$t/file.out" && ! grep -q 'history rewrite' "$t/file.out" || r=1; rm -rf "$t"; exit $r

## A pre-commit hook is not the enforcement boundary

A pre-commit hook is bypassable (`--no-verify`). The enforcement boundary is a **server-side/CI
gate** (push protection, or a CI job failing the build on any detected secret).

Source: [[knowledge-agent-architecture-combination-plan]] (§5.4)
Verified: 2026-10-12 · by: agent · method: manual
Recheck: t=$(mktemp -d); git init -q "$t/r"; mkdir -p "$t/r/.githooks"; printf "#!/bin/sh\nexit 1\n" > "$t/r/.githooks/pre-commit"; chmod +x "$t/r/.githooks/pre-commit"; git -C "$t/r" config core.hooksPath .githooks; printf x > "$t/r/f"; git -C "$t/r" add -A; if git -C "$t/r" -c user.email=t@t -c user.name=t commit -qm plain >/dev/null 2>&1; then r=1; elif git -C "$t/r" -c user.email=t@t -c user.name=t commit -qm bypass --no-verify >/dev/null 2>&1; then r=0; else r=1; fi; rm -rf "$t"; exit $r

The engine's `check-secrets` header states the same: "a pre-commit hook is bypassable with --no-verify, so the enforcement boundary is the server-side gate", and that the scan is "the committer-side half of the guardrail only". Of the committer-side scan and the server-side gate it says: "Both are required."

Source: [[script-check-secrets]]
Verified: 2026-10-12 · by: agent · method: doc-review

This workspace's current, concrete standing on both points — what is enforced, dormant, or
absent here specifically — is recorded in `ai-kb:documents/decisions/0002-governance-without-enforcement.md`,
not restated here; its amendment of 2026-10-02 is the current account. What that amendment adds that
holds generally: where a push-time rejection is available, it is the boundary only for the credential
shapes the hosting platform recognises, so the server-side workflow "remains necessary" for the
engine's own patterns, the entropy and identifier passes and the regression suite. A CI job that fails
after a push has landed is detection behind that boundary — "detection, not prevention" — and not a
substitute for it. A repository where neither is enforced is "recorded as unenforced rather than
described as protected".

Source: [[adr-0002-governance-without-enforcement]]
Verified: 2026-10-12 · by: agent · method: doc-review

## What check-secrets covers: three passes

`check-secrets` runs three passes per file, and any pass blocks:

1. the patterns in `secret-patterns.txt` — known credential shapes;
2. `secret-entropy` — "a heuristic for secret-labelled values and high-entropy tokens that have no recognisable shape";
3. `pii-scan` — "email addresses, domain-qualified Windows accounts, user home-directory paths", checked against `pii-allowlist.txt`.

"Put `lint-allow: <reason>` on a line to clear a legitimate match from all three." Standing identifiers — "role aliases, service accounts, team lists" — go in `pii-allowlist.txt` instead, "shared by every repo that runs this script". A credential hit is labelled `SECRET?` and a personal-identifier hit `PII?`; a `PII?` hit, the header says, needs no rotation: the fix is to remove the identifier.

It has four modes: staged files (the default); `--worktree`, "tracked files AND untracked, non-ignored ones (retrieval reads the tree, tracked or not)"; `--file PATH`, one file through the same three passes; and `--all`, every blob in full git history and the worktree.

The second pass has two rules. *Labelled*: a secret-ish label followed by `=` or `:` and a value of 8+ characters drawing on 3+ character classes. *Bare*: a single token of 16+ characters with a digit and 3+ classes including a symbol, or 20+ characters of mixed-case letters and digits, with Shannon entropy >= 3.5 bits/char (`-_.` count as glue, not symbols). It does not flag hex-only strings, URLs and paths, wiki-links and markdown links, ISO dates, hyphenated words and slugs, code, or placeholders.

The third pass has three rules: an email address; an uppercase domain-qualified account (a domain of 2-15 chars, a backslash, a user name); and a home-directory path under `/home/`, `/Users/`, or the Windows equivalent. A placeholder is not a name — `<user>`, `$USER`, `%USERNAME%` and `~` never match, and the generic names user, username, you, me, name, runner, public, default and shared are exempt. Given a directory rather than a file, `pii-scan` scans every file under it, skipping `.git`.

A `SECRET?` finding "shows its first three lines and says how many more there are".

Source: [[script-check-secrets]] · [[script-secret-entropy]] · [[script-pii-scan]]
Verified: 2026-10-16 · by: agent · method: doc-review

The behaviour was also run once against synthetic input in a scratch directory: a labelled password, an email address, a home path and a domain-qualified account each blocked under `--file`; the same password line carrying `lint-allow: <reason>` passed; an untracked file holding the password was caught by `--worktree`.

Source: [[script-check-secrets]]
Verified: 2026-10-12 · by: agent · method: manual
Recheck: s=$(cd ../../scripts && pwd) && t=$(mktemp -d) && r=0; p=Ab1#; printf 'k = AKIA%s\n' ABCDEFGHIJKLMNOP > "$t/a"; printf 'password = %s%s%s\n' "$p" "$p" "$p" > "$t/b"; printf 'mail a.person@%s\n' corp.invalid > "$t/c"; for f in a b c; do "$s/check-secrets" --file "$t/$f" >"$t/$f.out" 2>&1; [ $? -eq 1 ] || r=1; sed 's/$/ lint-allow: demo/' "$t/$f" > "$t/$f.ok"; "$s/check-secrets" --file "$t/$f.ok" >/dev/null 2>&1 || r=1; done; grep -q 'SECRET?' "$t/a.out" && grep -q 'SECRET?' "$t/b.out" && grep -q 'PII?' "$t/c.out" || r=1; rm -rf "$t"; exit $r

## Raw artifacts archived in a library

A library archives the evidence it was folded from under `documents/`, verbatim. The pilot that exposed the problem archived a schema dump of 1,383 SQL files plus diagram exports, and the bare rule flagged it about 1,900 times, nearly all on identifiers, while the pattern file found 0. The only escape was `lint-allow:` on every line, which would have meant editing thousands of lines of a verbatim artifact and blinding them to a future real secret.

Two exceptions in `check-secrets` cover such artifacts. A non-`.md` file under `documents/` in a library repository (one whose root `INDEX.md` declares `domain:`) skips the bare rule, "which flags its long identifiers on nearly every line. The patterns, the labelled rule and PII still run on every line of it." The engine's own `documents/` is not a library's, and keeps every rule; in `--file` mode "no file is exempt". UTF-16 text "reads as binary to every pass and so would be skipped unseen. It is reported `UNSCANNED` and blocks until converted to UTF-8."

What the exemption gives up, as the change states it: "a prefixless, unlabeled random literal inside an archived non-`.md` artifact is no longer caught." The contract's rule for archiving as UTF-8 is in `ai-kb:CONVENTIONS.md` §6 and is not restated here.

Source: [[script-check-secrets]] · [[pr-59-archived-artifacts]]
Verified: 2026-10-16 · by: agent · method: doc-review

## What check-secrets cannot see

The header states its limits plainly: "this is a pattern scan plus a heuristic, not a dedicated secret scanner. The heuristic is tuned to keep false positives low, so it will still miss short, low-entropy or oddly-shaped secrets." The entropy pass says of itself: "a heuristic. It will miss low-entropy or short secrets and may flag a legitimate literal".

One miss is structural rather than statistical, and the headers do not state it: "Tokens containing `/` are exempt, so a base64 secret that contains `/` is missed. This is the trade that keeps paths from being flagged." A secret is not caught for being random enough if it also looks like a path.

On identifiers: "The PII pass sees identifiers only: it cannot detect a personal NAME in prose — that is a human call at review." A name in prose therefore passes every scan, and only the human gate stops it. The personal-identifier pass also misses an account whose user name starts with an escape letter (n, t, r, f, v, a, b or 0), because it reads that as an escape sequence.

Source: [[script-check-secrets]] · [[script-secret-entropy]] · [[script-pii-scan]] · [[pr-6-secret-entropy-pass]]
Verified: 2026-10-12 · by: agent · method: doc-review

## Why a finding is recorded by reference

The rule — record a finding by reference, never by value; redact before archiving; name a role, not a person — is in `ai-kb:CONVENTIONS.md` §6 and is not restated here. What this library holds is why the rule is needed.

Consolidating a security finding is the step where a literal is most likely to be pasted: when a model distills a note that says a system had a plaintext password, "the natural (wrong) move is to paste the literal value as evidence". A knowledge base that does so has leaked the secret "through the front door, as documentation". A fold's ethic of copying load-bearing values character for character is the same instinct, which is why `kb-update-domain` carries an explicit carve-out: "faithful copying stops at the secret". The scan is the net behind the rule, not a substitute for it, and it leaves names to the human gate.

Source: [[issue-5-distill-secrets-gate]] · [[pr-8-distill-secrets-rule]]
Verified: 2026-10-12 · by: agent · method: doc-review

## The capture boundary: refuse before anything is written

`kb-capture` refuses a note holding a credential or a personal identifier. The reason is where retrieval looks: "Retrieval reads the working tree before any commit hook runs, so a note filed with a live password in it is routable, and quotable, from the moment it lands." A commit-time block arrives after the note has been filed, routed and possibly cited by distilled facts. The owner's decision was to refuse rather than warn, because a warning would let the note be filed anyway.

A refusal writes nothing and routes nothing, and exits 1. A draft typed in an editor or piped in is saved to a private (`0600`) file outside the tree, with the retry command printed; a draft given with `--from FILE` is left untouched. `lint-allow: <reason>` on a line clears a deliberate example, as it does everywhere else. For the same reason `check-secrets --worktree` scans untracked, non-ignored files, so a sweep sees what retrieval sees.

Source: [[script-kb-capture]] · [[issue-9-kb-capture-scan]]
Verified: 2026-10-12 · by: agent · method: doc-review

The refusal was also run once against synthetic input in a scratch tree: a note holding a labelled password was refused with the tree byte-identical afterwards, the `--from` file untouched, a piped draft saved with mode `-rw-------` outside the tree, and a clean note filed as normal.

Source: [[script-kb-capture]]
Verified: 2026-10-12 · by: agent · method: manual
Recheck: s=$(cd ../../scripts && pwd) && t=$(mktemp -d) && r=0; export GIT_CONFIG_GLOBAL=/dev/null GIT_CONFIG_NOSYSTEM=1; mkdir -p "$t/tree" "$t/tmp"; printf -- '---\nname: x\ndomain: d\nscope: general\n---\n# x\n' > "$t/tree/INDEX.md"; p=Ab1#; printf '## What happened\n\npassword = %s%s%s\n' "$p" "$p" "$p" > "$t/note"; before=$(cd "$t/tree" && find . -type f | sort | xargs cat | cksum); (cd "$t" && TMPDIR="$t/tmp" "$s/kb-capture" --into "$t/tree" --title demo --from "$t/note" >/dev/null 2>&1); [ $? -eq 1 ] || r=1; (cd "$t" && TMPDIR="$t/tmp" "$s/kb-capture" --into "$t/tree" --title demo2 --from - < "$t/note" >/dev/null 2>&1); [ $? -eq 1 ] || r=1; after=$(cd "$t/tree" && find . -type f | sort | xargs cat | cksum); [ "$before" = "$after" ] || r=1; [ ! -d "$t/tree/episodic" ] || r=1; d=$(ls "$t"/tmp/kb-capture-draft.* 2>/dev/null | head -1); [ -n "$d" ] && [ "$(stat -c %a "$d")" = 600 ] || r=1; rm -rf "$t"; exit $r

## Evidence a gate works: one blind trap test

The reference-not-value rule at the distill gate was tested once, blind, and the result is recorded in the description of the engine pull request that added it. An agent given only the skill file and the contract distilled a rigged episodic note that held a live-looking password, a colleague's name and email, a domain-qualified service account, a home-directory path, and some legitimate facts. It recorded the finding by vault reference, commit and rotate action; the account became "a service account" and the colleague "a team member". A grep of every distilled file found none of the bait outside the original note, and the speculation and the decision in the note were refused as facts. The distilled draft passed the project hook; the raw note was blocked by both the credential pass and the identifier pass.

This is a single run by one agent, reported by the change's author; it has not been re-run here. It also found a gap: the raw note held the secret from the moment `kb-capture` wrote it, and nothing flagged it until commit time. That gap is what the capture boundary above closes.

Source: [[pr-8-distill-secrets-rule]] · [[issue-9-kb-capture-scan]]
Verified: 2026-10-12 · by: agent · method: doc-review
