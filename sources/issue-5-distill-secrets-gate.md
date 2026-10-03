---
name: issue-5-distill-secrets-gate
description: Source stub — engine issue 5, why the distill gate must record a secret by reference, not by value.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-10-02
tags: [knowledge-architecture, source]
keywords: [issue, distill gate, secret by reference, front door, documentation, consolidation]
---
# Source: engine issue 5, Distill gate must enforce reference-by-name, never paste literals

- **Title:** Distill gate must enforce "reference secrets by name, never paste literals"
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** 2026-10-02 (closed by pull request 8)
- **Date ingested:** 2026-10-02
- **Artifact location:** `ai-kb#5` (the issue description), read via the hosting platform's command-line client on 2026-10-02. The description is editable text, not archived here: it is a public record in the engine repository.

The statement of the failure mode: consolidating a security finding is the step where pasting the literal is the natural, wrong move. Folded: that failure mode. Not folded: the proposed wording and acceptance criteria, which became the contract's rule.
