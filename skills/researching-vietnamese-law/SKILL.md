---
name: researching-vietnamese-law
description: Use when the user asks anything about Vietnamese law — conditions/procedures for something ("điều kiện chào bán chứng khoán", "what do I need to issue shares?"), whether a document is still valid ("Nghị định 58/2012 còn hiệu lực không?", "is decree X still in force?"), which Nghị định/Thông tư implements a Luật, or to quote a specific Điều. Drives the regai retrieval tools and the cite-correctly research loop.
---

# Researching Vietnamese Law with regai

## What this gives the user

regai is a curated, LLM-free knowledge base of Vietnamese legal documents. **You**
run the tools and synthesize a plain-language, correctly-cited answer; the user only
asks the question. They never type a command.

The corpus is curated and grows over time as documents are added from the remote, so
**don't assume what it covers from memory.** If a document genuinely isn't found, it
is outside the *current* set — say so plainly rather than guessing at its contents,
and never answer Vietnamese law from memory. To establish what the corpus actually
holds (e.g. before telling a user a topic is out of scope), use the
**checking-legal-coverage** skill.

## Core principle

Start wide, then narrow to verified text:

1. **`deep-research`** — entry point for any question; returns the full regulatory
   stack (Luật → Nghị định → Thông tư) at once.
2. **`check-in-force`** — before citing *any* document, confirm it's still valid.
3. **`get-article`** — read the actual Điều text before quoting it.
4. **`list-related`** — follow the regulation chain (which NĐ/TT implements a Luật).

Never quote a `search`/`deep-research` hit title without reading the body, and never
cite a document you haven't run through `check-in-force`.

→ Tool signatures and JSON shapes: **`reference/tools.md`**
→ How to turn a plain question into good search terms: **`reference/query-craft.md`**

## The canonical loop

```
1. deep_research(question="...")         → which documents & tiers are relevant
2. for each relevant document:
   a. check_in_force(slug=...)           → skip, or follow in_force_slug if superseded
   b. get_document(id=...)               → article ToC; pick which Điều to read
   c. get_article(doc=..., dieu=N)      → read the text before quoting
3. list_related(slug=..., relation="duoc_quy_dinh_chi_tiet_boi")
                                         → find implementing decrees/circulars
4. an article cites another doc by số hiệu? → get_document(id=<số hiệu>) to resolve
```

## Reading order — the Vietnamese legal hierarchy

Always read and present findings in this order:

| Tier | What to extract |
|------|----------------|
| **Luật** | Root obligation + scope (nghĩa vụ gốc, phạm vi điều chỉnh) |
| **Nghị định** | **Primary analysis** — conditions, procedures, exceptions, numeric thresholds |
| **Thông tư** | Operational compliance — forms, filing deadlines, technical standards, KPIs |
| **Hiến pháp** | Constitutional basis — only when a fundamental right (Điều 32–33) is at stake |

`deep-research` returns layers in this order; the **Nghị định is the primary layer** —
it carries the most actionable detail for practitioners.

## Citing sources

Always include **số hiệu + Điều number + tình trạng** (in force / expired).

- **Good:** "Theo Điều 15 Nghị định 155/2020/NĐ-CP (còn hiệu lực)…"
- **Bad:** "According to the regulation…" (no traceability)

## Top pitfalls

| Mistake | Fix |
|---------|-----|
| Quoting a search hit's title without the body | Call `get-article` first |
| Citing an expired document | Call `check-in-force`; cite `in_force_slug` instead |
| Treating empty `layers["luat"]` as "no law applies" | A Luật may govern without matching text; confirm with `search` |
| Jumping to `search` before `deep-research` | `deep-research` gives the whole stack — start there |

## When the tools won't run

If a tool can't reach the corpus, run `/regai-doctor` to check the connection, then
retry. If it still fails, tell the user — do **not** fall back to answering from memory.
