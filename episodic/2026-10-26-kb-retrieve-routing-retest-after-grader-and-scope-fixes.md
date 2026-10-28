---
name: 2026-10-26-kb-retrieve-routing-retest-after-grader-and-scope-fixes
description: "kb-retrieve routing retest after grader and scope fixes: Sonnet 130/138 at 6 libraries, 18/24 at 48; Haiku 127/138, 48-library round skipped"
memory_type: episodic
domain: knowledge-architecture
scope: general
metadata:
  type: episodic
  node_type: memory
  created: 2026-10-26
tags: [knowledge-architecture, episodic]
keywords: [episodic, session]
---
# 2026-10-26 — kb-retrieve routing retest after grader and scope fixes: Sonnet 130/138 at 6 libraries, 18/24 at 48; Haiku 127/138, 48-library round skipped

## What happened

Routing retest of `kb-retrieve` on 2026-10-26, run after engine PRs #129, #131 (grader fixes #121/#122), #132 (#124) and #133 (contract 2.3, #123) and the 12 library PRs (scope lines; 18 boundary golden cases, #126) were merged. All six libraries on main, `check-kb` clean on each ("28/21/13/15/20/47 files checked — clean").

Setup:
- Golden sets now hold 156 positive/cross-root cases (138 original + 18 boundary appended at the end: agile Q17–19, ai-writing Q22–24, azure Q18–20, ccr Q22–24, intel Q19–21, KA Q65–67). Original Q numbers unchanged.
- Samples prepared with `kb-eval-sample prepare kb/<lib> --seed 20261026 --n 999` (Sonnet) and `--seed 20261027` (Haiku); 48-library subsample with `--cases` matching the 2026-10-23 subsample.
- Each case run as a fresh `knowledge-tools:kb-retrieve` agent, description `kb-eval <sample_id> Q<n>`, prompt = question + the "Do not open golden sets…" line. Graded with `kb-eval-sample grade <dir>`.
- Haiku runs used the per-call Agent `model: haiku` override; agent definition left at `model: sonnet`. Transcripts showed `"model":"claude-haiku-5-5"`; meta.json `"model":"haiku"`.
- The plugin loads from the engine directory (known_marketplaces.json source `directory`, path `/mnt/c/workspaces/ws_v1/knowledge`), so contract 2.3 kb-retrieve was live.
- Concurrency cap hit: "Concurrent subagent limit reached. You can run 20 subagents at once." Two launches were refused and re-sent later.
- A first trial-grade showed agile Q18 "expected file 'story-format-and-invest.md' was never Read"; that run was still in progress at grade time. Re-graded later: Sonnet agile 16/16.

Results (raw, from grade.txt; summary script `summ.py` splits original vs boundary):
- Sonnet, 6 libraries, original 138: routing 130 (94.2%), grounding 129 (93.5%). Boundary 18: routing 17, grounding 17.
  Fails: azure Q14 (cross-root, read agile only); KA Q7, Q34, Q45, Q66* (content read from unneeded claude-code-runtime); KA Q35 ("expected file 'design-thesis.md' was never Read"); KA Q46, Q47, Q48 (answered from claude-code-runtime); KA Q45 also "contamination: Grep returned …/claude-code-runtime/claude-code-runtime-golden.md". Grounding-only fails: ccr Q8, Q12, Q13 ("excerpt not in answer").
- Haiku, 6 libraries, original 138: routing 127 (92.0%), grounding 121 (87.7%). Boundary: routing 16, grounding 17.
  Extra Haiku-only failures include KA Q21 and Q64 citing `/mnt/c/workspaces/ws_v1/knowledge/CONVENTIONS.md` instead of a library file (Q64: "no installed library was Read"), and KA Q52 "content read from unneeded agile-requirements, ai-writing-signals, azure-devops, claude-code-runtime, intelligence-analysis".
- 24-case subsample, 6 libraries (taken from the full runs): Sonnet routing 23/24, grounding 22/24 (fails ccr Q13 grounding, KA Q35). Haiku 23/24, 21/24.
- 24-case subsample, 48 libraries (42 decoys registered from `~/kb-libraries` with `kb-library add`), Sonnet only: routing 18/24, grounding 19/24.
  - azure Q6: "content read from unneeded agile-estimation" (grounded).
  - ccr Q21: read git-hooks only, answered "fact not found — check KB".
  - intel Q11, Q12: answered from red-team-analysis, cited `~/kb-libraries/red-team-analysis/devils-advocacy-and-pre-mortems.md`.
  - KA Q4: read git-hooks / shell-scripting-basics / ccr INDEX, answered "fact not found — check KB".
  - KA Q35: same miss as at 6 libraries.
- Haiku 48-library round not run: the agreed gate (routing ≥95%, grounding within 1–2 points of Sonnet) was not met at 6 libraries.

Decoy notes:
- Decoys rebuilt from session 2881045d's `routing/src` with `build.py` (OUT patched to `~/kb-libraries`); they declare `kb_contract: "2.2"` and carry no "Not covered here" line.
- With 48 registered, `kb-session-start` printed 42 "built to contract 2.2; this engine knows 2.3" nudges; `kb-session-start --subagent` context did not contain them ("behind" not in the 4215-char context).

Cleanup:
- Registry backed up first, sha256 `5079533fd70ef09770d0dba3eea5f1bdcfd019a525355afbcafb8aba7310ead9`; restored with `cp -p`; checksum matched; `~/kb-libraries` deleted; `kb-library list` shows the 6 real libraries ok.
- Graded the 48-library round before restoring the registry.

Follow-ups done the same day:
- Doc "ai-kb vs. Graph Knowledge Systems", section "Where the inflection points are": added a "Retest after the fixes" block and table (doc rev 24); the grader-problems line now says both were fixed in #131.
- ai-kb #125 closed as not planned with a comment folding it into #130; #130 got a comment carrying #125's read-both rule and the retest numbers.

## What we believe but did not check

- KA Q47 may be a golden-set design issue rather than an agent error: the question asks where a shell-startup fact is recorded, and agents went straight to claude-code-runtime:config-resolution, which is where it is recorded. Not adjusted in the scores; nobody has reviewed the golden record.
- The 94.2% Sonnet figure is probably "honest" in the sense that the two known grader artefacts are fixed, but no regrade review of the eight failures was done this session.
- The 2.3 scope lines did not visibly help against decoys, but the decoys have no scope lines of their own, so real neighbours built to 2.3 might separate better. Not tested.
- One trial per case: these numbers show direction, not frequency. The 18/24 vs 17/24 at 48 libraries difference is one case and could be noise.
- Haiku's reliance on CONVENTIONS.md looks like a model behaviour difference, but only two cases show it; not investigated.
- The freeze reasoning for folding #125 into #130 (shipping alone would land in the v0.3.0 freeze from 2026-10-29 and change the contract twice) is a planning judgement, not a measurement.
