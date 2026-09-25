<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="wordmark-dark.svg" />
    <img src="wordmark-light.svg" alt="regai" width="360" />
  </picture>
</p>

# regai: your in-house Vietnamese legal consultant (Claude Code plugin)

Do Vietnamese legal work in plain language, with answers you can actually rely on.
A general AI assistant answers from whatever it half-remembers from training, which is
often outdated, usually unsourced, and sometimes simply wrong. regai works differently.
Every answer is grounded in a curated corpus of Vietnamese law and cited the way a
careful lawyer cites (số hiệu + Điều + tình trạng). In-force status and superseding
documents are checked on every answer, never guessed.

## Install

This repo is a single-plugin Claude Code marketplace.

```
/plugin marketplace add infina-pm/regai-plugin
/plugin install regai@regai
```

(Or `/plugin marketplace add <path-to-this-repo>` for a local clone.)

When you enable the plugin, Claude Code asks you to approve the `regai` MCP server.
Approve it and you are done. There is no account, no API token, and nothing else to
configure: the plugin connects anonymously to a hosted, always-current corpus. It
ships no binaries and no secrets.

Run `/regai-doctor` any time to confirm the connection works.

## What you get

| Skill / command | Use it for |
| --- | --- |
| **`regai-live-check`** | Point questions against the live corpus: conditions, procedures, definitions, "is this still in force?", which Nghị định/Thông tư implements a Luật, quoting an Điều. |
| **`regai-listing-coverage`** | "Do you cover labour law?", "Is Nghị định 13/2023 in the corpus?" Checks coverage by probing the live corpus, never from memory. |
| **`regai-shipping-goal`** | Multi-step legal goals carried to completion: plan, approve, execute, deliver. |
| **`regai-onboard`** | A short interview that records your organization and house style, so every goal starts from your context. |
| **`regai-vault-search`** | Offline research over a bundled snapshot of the corpus, with no network. Also the fallback when the live tools are unreachable. |
| **`/regai-doctor`** | Checks that the MCP server is connected and the corpus answers. |

You never have to name a skill. Ask in plain language and Claude picks the right one.

## Ask a question

Ask a Vietnamese-law question the way you would ask a colleague:

- **Conditions / procedures**: *"Điều kiện chào bán chứng khoán ra công chúng là gì?"*
- **Definitions**: *"Tài khoản đảm bảo thanh toán là gì?"*
- **Is it still valid?**: *"Nghị định 58/2012 còn hiệu lực không?"*
- **Which rule implements a law**: *"Nghị định nào hướng dẫn Luật Chứng khoán 2019?"*
- **Quote a specific article**: *"Trích Điều 15 Nghị định 155/2020."*

Claude runs the research loop through the regai tools (search, read the article,
resolve the in-force version, walk down to the implementing documents) and returns a
plain-language answer with every claim cited.

### Why the answers hold up

The skills follow the discipline a careful Vietnamese legal researcher would:

1. **Reads the actual article text** before quoting anything. It never answers from a
   title or from memory.
2. **Resolves the in-force version.** If a document has been replaced
   (`bi_thay_the_boi`), annulled (`bi_bai_bo_boi`), or amended
   (`bi_sua_doi_bo_sung_boi`), it follows the link and cites the current document,
   even when the old one still reads "còn hiệu lực".
3. **Catches article-level amendments**, so it cites the amended text of an individual
   Điều rather than the superseded original.
4. **Reads in legal tier order**: Luật (scope), then Nghị định (conditions and
   thresholds), then Thông tư (forms and deadlines), and presents findings the same way.
5. **Cites everything** as số hiệu + Điều + tình trạng, and says plainly when the corpus
   does not cover a topic instead of inventing an answer.

## Carry a legal goal to completion

Give Claude a goal instead of a question, and `regai-shipping-goal` works like a legal
associate. It makes a plan, waits for your approval, then executes with checkpoints and
grounds every legal claim in the corpus. Built-in playbooks cover:

- Drafting a contract or an NDA
- Reviewing a contract or an NDA for risks
- Reviewing a product proposal
- Writing a legal memo (including a formal memo template)
- Mapping a compliance process

The first time you start a goal in a workspace, `regai-onboard` asks a few questions
about your organization and house style. The plugin keeps its working files in a
`.regai/` folder in your workspace:

- `.regai/context.md`: your organization and house-style profile
- `.regai/missions/`: the plan and output of each goal
- `.regai/memory/learnings.md`: what it learned, so later goals improve

## What the corpus covers

The corpus is curated and grows over time as new documents are published. It includes
securities and capital markets, banking and payments, insurance, accounting and tax,
enterprise and civil law, labour, cybersecurity, electronic transactions, artificial
intelligence, and the digital technology sector. Its scope changes, so ask Claude ("what do you cover?",
"is labour law in regai?") and `regai-listing-coverage` checks the live corpus.

## Working offline

The plugin also bundles a snapshot of the corpus (about 1,000 documents in Markdown,
under `skills/regai-vault-search/vault/`). The `regai-vault-search` skill researches it
entirely on your machine with grep and file reads, using the same research discipline
as the live skills.

Use it when you have no network, or when the live tools are unavailable. If the MCP
server cannot be reached, the other skills suggest switching to it instead of answering
from memory. The snapshot is frozen at the plugin's release, so for the latest
documents and statuses, prefer the live corpus.

## Troubleshooting

- **The regai tools are missing.** Check that the plugin is enabled and that you
  approved the `regai` MCP server, then reload Claude Code.
- **Tools time out or error.** Run `/regai-doctor`. It checks the hosted server at
  `https://psvpergdfbqzsagaacpz.supabase.co/functions/v1/mcp` and reports the error.
  In the meantime, continue with `regai-vault-search`.

## Try it

- "Điều kiện chào bán chứng khoán ra công chúng là gì?"
- "Nghị định 58/2012 còn hiệu lực không?"
- "Trích Điều 15 Nghị định 155/2020 cho tôi."
- "Review this service contract for legal risks under Vietnamese law."
- "Write a memo on the conditions for a company to issue bonds to the public."
