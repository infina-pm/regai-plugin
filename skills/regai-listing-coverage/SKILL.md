---
name: regai-listing-coverage
description: Use when the user asks what the regai knowledge base covers, whether a particular law/topic/document is in it ("do you have labor law?", "is Nghị định 13/2023 in the corpus?", "can you research tax?"), or before telling a user that a topic is out of scope. Determines coverage by probing the live corpus, never from memory — the corpus grows over time as documents are added from the remote.
---

# Checking what the regai corpus covers

## Why this is a skill

The corpus is **curated and grows over time** — documents are added from the remote,
so its scope is not fixed and must never be asserted from memory. There is no
catalog/stats tool among the six; coverage is established empirically by **probing the
live corpus** and reading what comes back. Use this before answering "what can I ask?"
or before declaring any topic out of scope.

Use the `regai` MCP tools the same way as the research tools — see
`../regai-checking-current-law/reference/tools.md` for argument signatures and JSON shapes.

## To check whether a specific topic or document is covered

1. **A specific document (số hiệu or slug)** → `check_in_force(slug=<id>)` (or
   `get_document(id=<id>)`). A `not_found` error means it's outside the current
   corpus; a result means it's in.
2. **A topic/area** → `search(query="<canonical terms for the topic>")` (see
   `../regai-checking-current-law/reference/query-craft.md` for term craft).
   - Substantive in-force hits → the area is covered; name it from the returned docs.
   - No hits, or only weak/expired/off-topic hits → likely **not** covered. Try one or
     two alternate canonical phrasings before concluding, then say it's not in the
     current corpus.
3. **Characterize what was found** with `get_document(id=<hit>)` and read its
   `nganh` (ngành / sector) and `linh_vuc` (lĩnh vực / field) — these name the legal
   area in the corpus's own terms, rather than your guess.

## To answer "what does the knowledge base cover?"

There is no list-everything call, so build the picture from probes:

1. Run `search` for a handful of candidate areas the user mentions or that are
   plausible, and note which return substantive in-force hits.
2. For the documents that come back, read their `nganh` / `linh_vuc` via
   `get-document` and group by those fields to describe the covered areas concretely.
3. Report what you **confirmed by probing**, and be explicit that the corpus grows —
   absence in a probe means "not found today," not "permanently unavailable."

## Reporting to the user

- Cover what you verified, in plain language, naming areas from `nganh`/`linh_vuc`,
  not from assumptions.
- If a topic isn't found: say it's outside the *current* corpus and that coverage is
  expanded centrally over time — don't speculate about contents you couldn't retrieve.
- If probes return nothing at all, the corpus may be unreachable — run `/regai-doctor`
  to check the connection before concluding it's empty.
