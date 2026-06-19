# regai tools — invocation & JSON shapes

Reference for the 6 retrieval tools. Load this when you need exact arguments or
output fields; the loop and judgment live in `../SKILL.md`.

## Calling the tools

These are **MCP tools** provided by the `regai` server (no shell, no binary). Call
them directly as tools; the user never types anything. The 6 tools are:
`search`, `get_article`, `get_document`, `list_related`, `check_in_force`,
`deep_research`. Each returns JSON; a missing target returns `{"error": "not_found"}`.

All `doc` / `id` / `slug` arguments accept **either** a vault slug
(e.g. `Luat-Chung-khoan-2019`) **or** a số hiệu (e.g. `155/2020/NĐ-CP`).

If the tools are unavailable, the regai MCP server is not connected — run
`/regai-doctor`.

## `deep_research` — layered topic research (entry point)

```
deep_research(question)
→ {
    question,
    reading_order: ["luat", "nghi_dinh", "thong_tu"],   # + "hien_phap" if surfaced
    primary_layer: "nghi_dinh",
    layers: {
      "luat":      {purpose: "...", items: [...]},
      "nghi_dinh": {purpose: "...", items: [...]},
      "thong_tu":  {purpose: "...", items: [...]},
      "hien_phap": {purpose: "...", items: [...]}        # only if a constitutional article is relevant
    },
    citations: [...],
    conflicts: [...]
  }
```
- Each item in `layers[tier].items`: `{doc_slug, so_hieu, tier, tinh_trang, ngay_co_hieu_luc, matched_dieu, score}`.
- `hien_phap` appears only when retrieval surfaces a constitutional article.

## `search` — find relevant articles

```
search(query, k=10, in_force_only=true)
→ {"hits": [{doc_slug, so_hieu, loai_van_ban, tier, tinh_trang, dieu, title, block_id, score}]}
```
- Higher `score` = more relevant.
- Set `in_force_only=false` only when researching history.
- Discovery only — read the body with `get_article` before quoting.
- Writing good queries: see `query-craft.md`.

## `get_article` — full text of one Điều

```
get_article(doc, dieu)
→ {doc_slug, so_hieu, dieu, title, body, block_id}
  | {error: "not_found", detail: "..."}
```
- `dieu` is the article number as a string: `15`, `3a`.

## `get_document` — metadata + article table of contents

```
get_document(id)
→ {doc_slug, so_hieu, loai_van_ban, tier, tieu_de, nganh, linh_vuc,
   ngay_co_hieu_luc, ngay_het_hieu_luc, tinh_trang,
   relations: {relation_name: [slug, ...]}, articles: [{dieu, title}]}
  | {error: "not_found"}
```
- Article `body` is **not** included — call `get_article` for text.
- `relations` shows the full graph edges (`duoc_quy_dinh_chi_tiet_boi`, `thay_the`, `bi_thay_the_boi`, …).

## `list_related` — follow the regulation graph

```
list_related(slug, relation=None, max_hops=1)
→ {"related": [{slug, so_hieu, tier, via_relation, hop}]}
  | {error: "not_found"}
```
- Omit `relation` to traverse all edges; pass one to narrow (e.g. `duoc_quy_dinh_chi_tiet_boi` = "is detailed by").

## `check_in_force` — temporal validity

```
check_in_force(slug)
→ {slug, tinh_trang, in_force_slug, superseded_by: [slug, ...]}
  | {error: "not_found"}
```
- `tinh_trang`:
  - `con_hieu_luc` — in force.
  - `het_hieu_luc_mot_phan` — **partially expired; still has live articles** (treat as live).
  - `het_hieu_luc` — fully expired.
- `in_force_slug` — the current valid document (differs from input if superseded).
- **Always call this before citing a document to a user.**
