---
name: regai-vault-search
description: Use when the user asks anything about Vietnamese law and you want to search the local vault/ folder directly (offline, no MCP/Supabase). Same research goals as regai-live-check but uses grep/Read over vault/ instead of the remote tools.
---

# Researching Vietnamese Law via Local Vault

## What this gives

A `vault/` folder of Markdown documents is the source of truth — every ingested
document lives there as `<slug>.md`. This skill drives **grep + Read** over those
files; it works fully offline and needs no MCP connection.

**Where the vault is.** The vault is bundled alongside this skill at `vault/` (relative to this skill's directory). Use that path everywhere below.

The corpus is whatever that snapshot contains. If a document isn't found, either it was
never ingested or the snapshot is stale — say so; never answer from memory.

## Core tools (bash/file, not MCP)

| Need | Command |
|------|---------|
| Find docs by keyword | `grep -ril "keyword" vault/` |
| Full-text search in a doc | `grep -n "điều\|keyword" vault/<slug>.md` |
| Read a document | Read `vault/<slug>.md` |
| Check hiệu lực | grep for `tinh_trang:` — live = `con_hieu_luc`/`het_hieu_luc_mot_phan` |
| Resolve in-force version | run the `get_in_force` algorithm by hand (see below) |
| Read successor links (dead docs) | `grep -nA3 "bi_thay_the_boi:"` — value is a list on the NEXT lines, not inline |
| Find article-level amendments | grep for `[!history]` blocks inside the doc |

## The canonical loop

```
1. grep -ril "<topic keywords>" vault/
   → collect candidate slugs

2. for each candidate, ALWAYS resolve to the current version (see below) BEFORE reading:
   a. Read vault/<slug>.md frontmatter
      - tinh_trang: het_hieu_luc → skip, follow the successor instead
      - bi_thay_the_boi: [[newer-slug]] → follow that slug
   b. grep -n "Điều N" vault/<slug>.md → find relevant articles
   c. Read the article text before quoting

3. list_related equivalent:
   grep -r 'duoc_quy_dinh_chi_tiet_boi:' vault/<slug>.md
   → follow wikilinks to implementing NĐ/TT slugs

4. số hiệu cited inside text?
   → grep -rl "<số hiệu>" vault/ to resolve to a slug
```

## Resolving "is this in force?" — the same rule the engine uses

Apply this rule to each doc, reading only its frontmatter, in this order:

1. **Has a successor? (`bi_thay_the_boi`, `bi_bai_bo_boi`, *or* `bi_sua_doi_bo_sung_boi`
   present)** → **always follow the successor regardless of `tinh_trang`.** Any of
   these — replaced, annulled, or amended — overrides a stale-live status (a doc can
   read `con_hieu_luc` yet already be superseded because the status was never flipped).
   Open the successor and apply this same rule to it.
2. **Dead? (`tinh_trang: het_hieu_luc`)** → ignore this doc; there's no successor to
   follow, so report no in-force version. (`het_hieu_luc` is the only status that makes
   a doc dead — `het_hieu_luc_mot_phan` still has active articles and is usable.)
3. **Otherwise** (live with no successor — `con_hieu_luc`, `het_hieu_luc_mot_phan`, or
   `chua_co_hieu_luc`) → **this is the in-force version; cite it as-is.**

Repeat until you land on a doc you cite. Watch for loops; if a chain circles back to a
slug you already visited, stop. If a superseded/dead doc has no live successor, tell
the user there is no in-force version — do not answer from memory.

### Reading the fields correctly

These are **multi-line YAML block lists** — the key is on one line, the `[[wikilinks]]`
on the lines below:

```yaml
bi_thay_the_boi:
- '[[TT-40-2024-NHNN]]'
```

⚠️ A bare `grep -E "^bi_thay_the_boi:"` matches only the key line and looks empty even
when populated. Use `grep -nA3` or just Read the frontmatter block:

```
grep -nA3 -E "^(tinh_trang|bi_thay_the_boi|bi_bai_bo_boi|bi_sua_doi_bo_sung_boi):" vault/<slug>.md
```

### Article-level amendments

The in-force rule above resolves *whole-doc* succession only. A doc that is live can
still have individual Điều amended — the vault records this in `[!history]` callout
blocks. Check them before quoting an article:

```
grep -n "\[!history\]" vault/<slug>.md   # e.g. "> Sửa đổi bởi [[TT-41-2025-NHNN#^dieu-27]]"
```

If an article was amended, read and cite the amending doc's version of that Điều.

## Reading order — the Vietnamese legal hierarchy

Always read and present findings in this order:

| Tier | What to extract |
|------|----------------|
| **Luật** | Root obligation + scope |
| **Nghị định** | **Primary analysis** — conditions, procedures, thresholds |
| **Thông tư** | Operational compliance — forms, deadlines, technical standards |
| **Hiến pháp** | Constitutional basis — only when a fundamental right is at stake |

## Vault document structure

Each `vault/<slug>.md` has:
- YAML frontmatter: `so_hieu`, `loai_van_ban`, `tinh_trang`, `ngay_co_hieu_luc`,
  `relations` (wikilinks), `bi_thay_the_boi`, `thay_the`, etc.
- Article headings: `## Điều N. Title ^block-id`

`tinh_trang` values: `con_hieu_luc`, `het_hieu_luc`, `het_hieu_luc_mot_phan`,
`chua_co_hieu_luc`. **Live = `con_hieu_luc` OR `het_hieu_luc_mot_phan`.**

## Citing sources

Always include **số hiệu + Điều number + tình trạng**.

- **Good:** "Theo Điều 15 Nghị định 155/2020/NĐ-CP (còn hiệu lực)…"
- **Bad:** "According to the regulation…"

## Synthesizing the final response

After retrieval, always produce a plain-language answer — never dump raw Markdown or
frontmatter at the user.

**If the user asked a question** (điều kiện, thủ tục, có được không…):
1. State the direct answer in one sentence first.
2. Walk through the legal basis tier by tier (Luật → Nghị định → Thông tư), quoting
   the key Điều with số hiệu + tình trạng.
3. Flag anything that is expired or partially expired (`het_hieu_luc_mot_phan`) and
   note which articles are still active.
4. Close with a one-line practical takeaway.

**If the user asked for a definition** (X là gì? what is Y?):
1. Give the statutory definition verbatim (or paraphrased if the text is dense),
   attributed to the exact Điều and document.
2. If multiple tiers define or refine the term, present them in reading order.
3. Note the effective date and whether the defining document is still in force.

**Format rules:**
- Write in the same language the user used (Vietnamese question → Vietnamese answer).
- Bold the số hiệu on first reference: **Nghị định 155/2020/NĐ-CP**.
- Use bullet points for lists of conditions or steps; prose for definitions.
- Never say "according to the regulation" without naming the document.
- If vault search returned nothing useful, say so explicitly — do not answer from memory.

## Top pitfalls

| Mistake | Fix |
|---------|-----|
| Quoting grep hits without reading the body | Read the article text first |
| Citing a doc with any of `bi_thay_the_boi`/`bi_bai_bo_boi`/`bi_sua_doi_bo_sung_boi` | Superseded — follow the successor even if `tinh_trang` reads live |
| Bare `grep "^bi_thay_the_boi:"` shows it empty | It's a multi-line list — use `grep -A3` or Read the frontmatter |
| Citing a live doc whose Điều was amended | Check `[!history]` blocks; cite the amending doc's article |
| Treating zero grep results as "no law applies" | Try synonyms / alternate spellings |
| Reading only the first hit | Check tier order; Nghị định carries the most detail |
| Dumping raw vault text at the user | Always synthesize into a plain-language answer |
