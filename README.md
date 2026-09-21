<p align="center">
  <img src="wordmark.png" alt="regai" width="420" />
</p>

# regai — your in-house Vietnamese legal consultant (Claude Code plugin)

Do Vietnamese legal work in plain language, with answers you can actually rely on.
A general AI assistant answers from whatever it half-remembers from training — often
outdated, usually unsourced, sometimes simply wrong. regai is built differently: every
answer is grounded in a curated corpus of Vietnamese law and cited the way a careful
lawyer cites — số hiệu + Điều + tình trạng — with in-force status and superseding
documents checked, never guessed.

## Install

This repo is a single-plugin Claude Code marketplace.

```
/plugin marketplace add infina-pm/regai-plugin
/plugin install regai@regai
```

(Or `/plugin marketplace add <path-to-this-repo>` for a local clone.)

That's it — the free **`regai-vault-search`** skill works immediately, fully offline.
No account, no network, no setup.

## Free use — offline legal research over the bundled corpus

The plugin ships with a snapshot of the regai corpus (`vault/`, ~800+ Vietnamese legal
documents in Markdown). The **`regai-vault-search`** skill researches it entirely on
your machine with grep + file reads — no MCP, no network, no API token.

### How to use it

Just ask a Vietnamese-law question in plain language. Claude invokes the skill and
drives the research loop for you:

- **Conditions / procedures** — *"Điều kiện chào bán chứng khoán ra công chúng là gì?"*
- **Definitions** — *"Tài khoản đảm bảo thanh toán là gì?"*
- **Is it still valid?** — *"Nghị định 58/2012 còn hiệu lực không?"*
- **Which rule implements a law** — *"Nghị định nào hướng dẫn Luật Chứng khoán 2019?"*
- **Quote a specific article** — *"Trích Điều 15 Nghị định 155/2020."*

### What it does under the hood (and why it's reliable)

The skill follows the same discipline a Vietnamese legal researcher would — by hand,
against the document frontmatter:

1. **Finds candidates** by keyword across the whole corpus, then reads the actual
   article text before quoting anything (never answers from a title or from memory).
2. **Resolves the in-force version.** Before citing any document it checks the
   succession fields — if a doc has been replaced (`bi_thay_the_boi`), annulled
   (`bi_bai_bo_boi`), or amended (`bi_sua_doi_bo_sung_boi`), it follows the link to the
   current document and cites *that*, even when the old one still reads "còn hiệu lực".
3. **Catches article-level amendments** via the `[!history]` blocks the vault records,
   so it cites the amended text of an individual Điều, not the superseded original.
4. **Reads in legal tier order** — Luật (scope) → Nghị định (the conditions and
   thresholds) → Thông tư (forms, deadlines) — and presents findings the same way.
5. **Cites everything** as số hiệu + Điều + tình trạng, and says so plainly when the
   corpus doesn't cover a topic instead of inventing an answer.

The corpus is a **snapshot frozen at the plugin's release** — great for offline,
reproducible research. For an always-current corpus, see Pro below.

## Pro

Pro connects regai to a hosted, **always-current** corpus and adds the full associate
workflow — live coverage, in-force resolution against the latest documents, and
goal-driven drafting/review/memo/compliance playbooks that learn your house style.

Pro features require access from the regai maintainer. Ask your maintainer to enable
Pro and provide setup details.

## Try it

- "Điều kiện chào bán chứng khoán ra công chúng là gì?"
- "Tài khoản đảm bảo thanh toán là gì?"
- "Nghị định 58/2012 còn hiệu lực không?"
- "Trích Điều 15 Nghị định 155/2020 cho tôi."
