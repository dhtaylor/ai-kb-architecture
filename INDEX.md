---
name: knowledge-architecture-index
description: Router for ai-kb, the governed knowledge-base engine, as it stands — installing and using it, the contract's rules, what every script, skill, agent and command does, the decisions behind them, and why the knowledge base and its thin-agent layer are designed as they are.
memory_type: reference
domain: knowledge-architecture
scope: general
freshness_horizon: 90d
agent_ttl: 30d
verifier_budget: 15
sweep_interval: 30d
kb_contract: "2.6"
last_swept: 2026-11-12
metadata:
  type: index
  node_type: router
  created: 2026-09-19
tags: [knowledge-architecture]
keywords: [ai-kb, install, set up, kb-bootstrap, kb-init, commit hook, contributing, CONVENTIONS, contract rules, frontmatter, currency stamp, script reference, exit codes, skills, commands, decision records, ADR, KB_TODAY, XDG, fixed paths, local-only, knowledge base, architecture, thin agent, retrieval contract, scoping, provenance, currency, lifecycle, guardrails, librarian, retrieval eval, golden set, oracle, release, contract version, kb_contract, links_to, read-only, dependency mismatch, unattended, review packet, schedule, section read, scope line, redact]
---
# Knowledge architecture

Not covered here: Claude Code's own runtime mechanisms, such as its settings, its hook system, plugin loading and tool semantics (the engine's own hooks, and how ai-kb uses Claude Code, are covered here) — held by a separate claude-code-runtime library, not published with this one

- [design-thesis](design-thesis.md) — why knowledge and behavior are separated, the knowledge-as-behavior anti-pattern, the three repository kinds, library registration and its states, a local-only library, the engine's files at fixed paths (XDG not read), the one reader of a root INDEX.md
- [releases-and-contract-versioning](releases-and-contract-versioning.md) — engine semantic versioning and the two-day freeze, the library contract's own MAJOR.MINOR version, contract versions 2.0 to 2.6, pinning as the library owner's business, installing someone else's library: read-only and pinned to a release tag with --ref, links_to and dependency mismatches, a local-only library's origin as its path, export and import carrying read-only and the pin
- [agent-lifecycle-roles](agent-lifecycle-roles.md) — the Curator, Research librarian and Fact-checker roles, the Verifier (its default budget, what it reads, weak assertions), Watcher and URL liveness check (request timeout, sign-in redirects, Liveness: manual) as built, what the nudge counts and hides, when session start shows it, mutation governance, a fact file holds current state, agent TTL, the session-start nudge, why no orchestrator is built and triage is proposed
- [unattended-preparation](unattended-preparation.md) — why preparing may run unattended while deciding stays attended: review packets (kept in done/ once closed), the weekly report (what it lists and what it does when it cannot read a library), the opt-in schedule, schedulable commands, the verified backup
- [retrieval-agent-behavior](retrieval-agent-behavior.md) — how the retrieval agent decides where to look: what counts as the knowledge base, section reads, scoped search, the scope line, the subagent library list, the triage desk and one specialist per library, files-first search and the stop rule
- [secret-governance](secret-governance.md) — why rotation is not removal, why a pre-commit scan is not the boundary, the three-pass scan and what it cannot see, dotted names, `--file` mode and archived artifacts, capture refusal, the redaction filter
- [guardrail-verification](guardrail-verification.md) — why a guardrail correct on the authoring machine fails elsewhere: defects found only by running or by a fresh reader, recurring defects, tests that pass for the wrong reason, the suite's cleared variables, KB_TODAY, a check given the wrong folder
- [retrieval-eval-integrity](retrieval-eval-integrity.md) — why a retrieval eval's score can mean nothing about the knowledge: a reachable oracle, grader defects, a moved protocol, one trial per case, cost and sample measurement, the runs that found nothing
- [using-ai-kb](using-ai-kb.md) — how to install and use ai-kb: setting up a machine, kb-bootstrap with no flag, --check and --apply, why every clone needs kb-init, registering a library (add, list, export and import, read-only, --ref), session start and the kb.nudge setting, the capture, distill and retrieve loop, adding a library, what to do when a commit goes unchecked, the engine's own hooks and test suite, contributing a change, how the engine resolves its root (ADR-0001); the steps a person takes, not what each script's options mean
- [skills-agents-and-commands](skills-agents-and-commands.md) — what the plugins hold and how a consumer registers them (ADR-0006: explicit registration, directory source, shadowing), the tier each file declares, and for each of kb-ask, kb-retrieve, kb-update-domain, kb-create-domain, kb-distill, kb-organize-domain, kb-audit, /kb-capture, /kb-init and intelligence-analysis: what it is for, which neighbor to use instead, what it refuses, where it stops for a person and which scripts it runs
- [script-reference-checks](script-reference-checks.md) — per-script reference for every check-* script, grade-eval, grade-routing, pii-scan and secret-entropy: purpose, usage as written, defaults, exit codes and what each never does (check-blast, check-embedded-facts, check-exec-bits, check-golden, check-kb, check-scope, check-secrets, check-stamps, check-tool-scope, check-transient, check-xlinks); not the kb-* tools, and not why a check exists, which the design files hold
- [script-reference-tools](script-reference-tools.md) — per-script reference for every kb-* script (kb-apply, kb-backup, kb-bootstrap, kb-capture, kb-catalog, kb-due, kb-eval-sample, kb-init, kb-libraries, kb-library, kb-owner, kb-prepare, kb-report, kb-schedule, kb-session-start, kb-sources, kb-telemetry, kb-verify, kb-watch) and the kbengine package: usage, defaults, exit codes; not the check-* scripts
- [contract-layout-and-naming](contract-layout-and-naming.md) — the contract's rules for where a fact lives and what a library looks like: scope values and the product-versus-general swap test, the comparative product library, the three homes, a root INDEX.md's declared fields, slugs and cross-library links, the frontmatter schema, naming, ADR-0003, ADR-0004 and ADR-0010
- [contract-provenance-and-currency](contract-provenance-and-currency.md) — the contract's rules for where a fact came from and whether it is still true: Source lines, the Verified stamp and Recheck assertions, trust tiers, the verified floor, CONFLICTED and UNCORROBORATED markers, recording a secret by reference, promotion to the general tier
- [contract-retrieval-and-verification](contract-retrieval-and-verification.md) — the contract's rules for retrieval and checking: what an answer must cite and quote, the golden set's record fields, case types and excerpt rules, and the list of the twenty-eight checks that verify a domain
- [sources](sources/INDEX.md) — citation stubs for the artifacts these facts came from
- [episodic](episodic/INDEX.md) — session notes, newest first: the raw record facts are distilled from
- [knowledge-architecture-open-questions](knowledge-architecture-open-questions.md) — gaps the sources left open, recorded rather than filled
- [needs-attention](needs-attention.md) — the queue of findings from automated checks and sweeps, resolved items kept as history; detection only
