---
name: pr-6-secret-entropy-pass
description: Source stub — engine pull request 6, the heuristic pass that blocks shapeless credentials, and the project-tree hook's secret scan.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-10-02
tags: [knowledge-architecture, source]
keywords: [pull request, check-secrets, secret-entropy, heuristic, base64, known limits, project hook]
---
# Source: engine pull request 6, check-secrets: block shapeless credentials; scan project trees for secrets

- **Title:** check-secrets: block shapeless credentials; scan project trees for secrets
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** 2026-10-02 (merged)
- **Date ingested:** 2026-10-02
- **Artifact location:** `ai-kb#6` (the pull request description), read via the hosting platform's command-line client on 2026-10-02. The description is editable text, not archived here: it is a public record in the engine repository, and quoting it carries no personal identifier.

The change record for the entropy pass. Folded: one known limit the script headers do not state — tokens containing `/` are exempt, so a base64 secret containing `/` is missed. Not folded: the pass's rules and thresholds (the `secret-entropy` header is the source for those), the test counts, and the project-hook wiring.
