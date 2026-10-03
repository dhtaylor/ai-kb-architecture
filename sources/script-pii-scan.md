---
name: script-pii-scan
description: Source stub — header of the engine's pii-scan script and its allowlist file, the personal-identifier pass.
memory_type: reference
domain: knowledge-architecture
scope: general
metadata:
  type: source
  node_type: citation
  created: 2026-10-02
tags: [knowledge-architecture, source]
keywords: [pii-scan, pii-allowlist, personal identifier, email, account, home directory, names]
---
# Source: header of the engine's pii-scan script and pii-allowlist file

- **Title:** pii-scan and pii-allowlist.txt (script docstring and file header)
- **Author:** the engine's maintainer (named in the artifact; not recorded here — this library is public)
- **Date:** undated in the artifact
- **Date ingested:** 2026-10-02
- **Artifact location:** `ai-kb:scripts/pii-scan` (docstring) and `ai-kb:scripts/pii-allowlist.txt` (header comment), read at engine commit `35f0e95` on 2026-10-02. Not archived in this library: it is a tracked, versioned document in the engine repository, and a copy would carry the maintainer's name, which this public library does not record.

Source for the third check-secrets pass: its three rules, its exemptions, the allowlist's purpose, and its stated limits. Folded: the three kinds of identifier, that names are not detectable, and the two stated misses. The allowlist's own entries are not folded.
