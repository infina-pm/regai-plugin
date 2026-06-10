---
name: researching-vietnamese-law
description: Use when answering Vietnamese legal questions using the regai knowledge base. Covers the 6 regai tools (search, get_article, get_document, list_related, check_in_force, deep_research), the canonical agentic research loop, the Vietnamese legal tier reading order, and how to cite sources correctly.
---

# Researching Vietnamese Law with regai

## Overview

regai is an LLM-free legal retrieval engine. You drive the research loop — calling tools, following leads, deciding when you have enough. regai returns citable legal data; you synthesize the answer.

**Core principle:** Start wide (`deep_research`), verify status (`check_in_force`), read specific text (`get_article`), follow the regulation chain (`list_related`).

## The 6 tools

All `id` / `slug` / `doc` parameters accept **either** a vault slug (e.g. `ND-155-2020`) **or** a số hiệu (e.g. `155/2020/NĐ-CP`).

### `search` — find relevant articles
```
search(query, k=10, in_force_only=True)
→ {"hits": [{doc_slug, so_hieu, loai_van_ban, tier, tinh_trang, dieu, title, block_id, score}]}
```
- Hybrid BM25+vector over article bodies.
- `in_force_only=True` (default) excludes expired documents. Set `False` only when researching history.
- Use for discovery. Scores are meaningful — higher = more relevant.

### `get_article` — full text of one Điều
```
get_article(doc, dieu)
→ {doc_slug, so_hieu, dieu, title, body, block_id}
  | {error: "not_found", detail: "..."}
```
- `dieu` is the article number as a string: `"15"`, `"3a"`.
- Call this after `search` or `deep_research` to read the actual text before quoting it.

### `get_document` — metadata + article table of contents
```
get_document(id)
→ {doc_slug, so_hieu, loai_van_ban, tier, tieu_de, nganh, linh_vuc,
   ngay_co_hieu_luc, ngay_het_hieu_luc, tinh_trang,
   relations: {relation_name: [slug, ...]},
   articles: [{dieu, title}]}
  | {error: "not_found"}
```
- Article `body` is **not** included — call `get_article` for text.
- `relations` shows the full graph edges: `duoc_quy_dinh_chi_tiet_boi`, `thay_the`, `bi_thay_the_boi`, etc.

### `list_related` — follow the regulation graph
```
list_related(slug, relation=None, max_hops=1)
→ {"related": [{slug, so_hieu, tier, via_relation, hop}]}
  | {error: "not_found"}
```
- BFS from the document over relation edges.
- `relation=None` traverses all edges; pass a relation name to narrow (e.g. `"duoc_quy_dinh_chi_tiet_boi"`).
- Use to find which Nghị định implements a Luật, or which document amended another.

### `check_in_force` — temporal validity
```
check_in_force(slug)
→ {slug, tinh_trang, in_force_slug, superseded_by: [slug, ...]}
  | {error: "not_found"}
```
- `tinh_trang`: `"con_hieu_luc"` (in force) or `"het_hieu_luc"` (expired).
- `in_force_slug`: the current valid document (may differ from input if superseded).
- **Always call this before citing a document to a user.**

### `deep_research` — layered topic research
```
deep_research(question)
→ {
    question,
    reading_order: ["luat", "nghi_dinh", "thong_tu"],  # + "hien_phap" if surfaced
    primary_layer: "nghi_dinh",
    layers: {
      "luat":      {purpose: "Nghĩa vụ gốc + phạm vi điều chỉnh",          items: [...]},
      "nghi_dinh": {purpose: "Điều kiện, quy trình, ngoại lệ, ngưỡng số liệu", items: [...]},
      "thong_tu":  {purpose: "Biểu mẫu, deadline, tiêu chuẩn kỹ thuật, KPI tuân thủ", items: [...]},
      "hien_phap": {purpose: "Cơ sở hiến định — quyền cơ bản Điều 32, 33",  items: [...]}
    },
    citations: [...],
    conflicts: [...]
  }
```
- Each item in `layers[tier]["items"]` has: `{doc_slug, so_hieu, tier, tinh_trang, ngay_co_hieu_luc, matched_dieu, score}`.
- `hien_phap` appears **only** when retrieval surfaces a constitutional article — no LLM judgment, fully deterministic.
- Use as your **entry point** for any legal question. It gives the full regulatory stack at once.

## Canonical research loop

```
1. deep_research(question)
      → identifies which documents and tiers are relevant

2. For each relevant document:
   a. check_in_force(slug)          → skip or follow successor if expired
   b. get_document(slug)            → see article ToC; pick which Điều to read
   c. get_article(slug, dieu)       → read the actual text before quoting

3. To find implementing decrees/circulars:
   list_related(slug, relation="duoc_quy_dinh_chi_tiet_boi")

4. If an article references another document by number:
   search("...") or get_document(so_hieu) to resolve it
```

## Reading order — Vietnamese legal hierarchy

Always read and present findings in this order:

| Tier | What to extract |
|------|----------------|
| **Luật** | Root obligation + scope (nghĩa vụ gốc, phạm vi điều chỉnh) |
| **Nghị định** | **Primary analysis**: conditions, procedures, exceptions, numeric thresholds |
| **Thông tư** | Operational compliance: forms, filing deadlines, technical standards, KPIs |
| **Hiến pháp** | Constitutional basis — only when a fundamental right (Điều 32-33) is at stake |

`deep_research` returns layers in this order. The Nghị định is the `primary_layer` — it has the most actionable detail for practitioners.

## Citing sources

Always include: `so_hieu`, the Điều number, and the `tinh_trang` (in force / expired).

**Good cite:** "Theo Điều 15 Nghị định 155/2020/NĐ-CP (còn hiệu lực)..."
**Bad cite:** "According to the regulation..." (no traceability)

## Common mistakes

| Mistake | Fix |
|---------|-----|
| Quoting `search` hit title without reading body | Call `get_article` first |
| Citing an expired document | Call `check_in_force`; use `in_force_slug` instead |
| Using `deep_research` results without checking tiers | Nghị định is primary; Luật sets the frame |
| Treating empty `layers["luat"]["items"]` as "no law applies" | A Luật may not have matched text but still governs; use `search` to confirm |
| Jumping to `search` without `deep_research` | `deep_research` gives the full stack; start there |

## Running the tools (plugin binary)

You call the bundled `regai` binary on the user's behalf — the user never types
commands. Pick the binary for the platform under `${CLAUDE_PLUGIN_ROOT}/bin/`:

- macOS Apple Silicon: `${CLAUDE_PLUGIN_ROOT}/bin/regai-macos-arm64`
- Windows x64: `${CLAUDE_PLUGIN_ROOT}/bin/regai-windows-x64.exe`

Detect the platform once (`uname -sm` on Unix). The binary reads the corpus from
`~/.regai/sec.db` by default, so you do **not** need to pass `--db`.

```bash
<binary> deep-research --question "điều kiện chào bán chứng khoán ra công chúng"
<binary> search        --query "vốn điều lệ tối thiểu"
<binary> get-article   --doc 155/2020/NĐ-CP --dieu 15
<binary> get-document  --id Luat-Chung-khoan-2019
<binary> list-related  --slug Luat-Chung-khoan-2019
<binary> check-in-force --slug 58/2012/NĐ-CP
```

All commands print JSON to stdout and exit 1 on a `not_found` error.

**If the corpus is missing** (the binary errors that it can't open `~/.regai/sec.db`),
run `<binary> update` first, then retry.

## Extracting good search keywords (important — keyword search only)

This corpus uses **keyword (BM25/FTS) search, not semantic vectors**. The quality
of `search`/`deep_research` depends on the terms you feed it. Before searching:

1. **Pull the legal nouns/verbs**, drop filler. From "Công ty tôi muốn phát hành
   thêm cổ phiếu cho cổ đông hiện hữu thì cần điều kiện gì?" extract
   `phát hành cổ phiếu cổ đông hiện hữu điều kiện`.
2. **Recognize a số hiệu** and search it directly — patterns like `155/2020/NĐ-CP`,
   `54/2019/QH14`. A số hiệu match pulls the exact document.
3. **Use canonical legal terms**, not colloquial ones: prefer `chào bán chứng khoán
   ra công chúng` over "bán cổ phiếu", `vốn điều lệ` over "tiền vốn",
   `người nội bộ` over "sếp công ty".
4. **Diacritics:** the index folds diacritics, so both `dieu kien` and `điều kiện`
   match — but always prefer correct Vietnamese spelling for precision.
5. **If hits are thin or off-topic, broaden then narrow:** start with 2–3 core terms;
   if too few results, drop the most specific term; if too noisy, add a distinguishing
   term (e.g. add `nghị định` or the domain like `chứng khoán`).
6. **Follow leads with the graph**, not more keyword guessing: once you have one
   relevant document, use `list-related` and `get-document` relations to reach its
   implementing decrees/circulars rather than re-searching.

## Corpus currency

- The corpus is a curated snapshot. Tell the user the coverage when relevant
  (the `update` output and `~/.regai/version.json` carry the version + a
  `corpus_note`).
- **If a document genuinely isn't found:** first suggest `/regai-update` (it may be
  newly published). If it's still absent after updating, tell the user it is outside
  the curated corpus rather than guessing at its contents.
