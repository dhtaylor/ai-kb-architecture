---
name: adr-0020-published-libraries-and-consumers
description: Source stub — engine ADR-0020, a library says where the libraries it links to come from, and a consumer's copy is read-only.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-11-09
tags: [knowledge-architecture, source]
keywords: [links_to, dependency mismatch, read-only, consumer, published library, origin, same_origin, kb-library add, --read-only, --ref, aliasing, contract 2.6]
---
# Source: ADR-0020: A library says where the libraries it links to come from, and a consumer's copy is read-only

- **Title:** ADR-0020: A library says where the libraries it links to come from, and a consumer's copy is read-only
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** 2026-11-09
- **Date ingested:** 2026-11-09
- **Artifact location:** `ai-kb:documents/decisions/0020-published-libraries-and-consumers.md` at tag `v0.4.0` (commit `a318c28`). First read at engine commit `5fb833a` on 2026-11-09. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.

The engine's decision record for libraries someone else publishes. Folded from it: the three problems it names, the options weighed, `links_to` and how origins compare, how a mismatch is reported, `kb-library add`'s warnings, the read-only role, the planned pinning, the consequences and what it leaves uncovered. 
- **Re-read:** on 2026-11-11 at engine release-branch commit `6c2197e` (`release-v0.4.0`, local, not yet merged to `main` when read), where the Delivery list puts `--ref` pinning in v0.4.0 and a dated note records that the owner moved it there ("Amended 2026-11-11 by the owner: `--ref` pinning ships in v0.4.0 with steps 1 and 2"). At `main` commit `dfa93f9` the same list says "(after v0.4.0)", which the amendment supersedes; the release-branch commit is cited for the timing because it is the only place the timing is stated as shipped. The "Pinning a consumer's copy" paragraph is unchanged between the two and still describes what was built.
- **Re-pinned:** to tag `v0.4.0` on 2026-11-12, after the release. `git diff 6c2197e v0.4.0` shows no change to the decision record, so the tag now states the timing as shipped and the release-branch commit is no longer the only place it is stated. The facts that cite this stub keep their stamps: nothing they cite changed.

Folded now also: the pinning as delivered, from the change notes and the script, with the decision's own statement that moving to a newer release is a person's step. What was built of it is cited from the engine's change notes ([[engine-changelog-v0.4.0]]), not from the decision.
