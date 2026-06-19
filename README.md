# regai — Vietnamese Legal Research (Claude Code plugin)

Research Vietnamese law in plain language. Ask questions in Vietnamese or English —
Claude drives a deterministic, **LLM-free** retrieval engine over a curated legal
corpus and answers with citable sources. Nothing decides the law for you; the tools
return legal data and Claude synthesizes from it.

The corpus (securities law and connected documents) is hosted centrally and kept up
to date. There's nothing to download or build — the plugin connects to the hosted
service over MCP.

## Install

This repo is a single-plugin Claude Code marketplace.

```
/plugin marketplace add infina-pm/regai-plugin
/plugin install regai@regai
```

(Or `/plugin marketplace add <path-to-this-repo>` for a local clone.)

On enable, Claude Code prompts you to:

1. **Approve the remote MCP server** (`regai` over HTTP).
2. **Enter your `api_token`** — the Bearer token for the hosted service. Ask the
   regai maintainer for one.

Then verify with `/regai-doctor` or just ask: *"check the regai connection."*

The hosted service is `https://regai-dn2n.onrender.com` (baked into the plugin). To
point at a local dev server instead, edit `mcpServers.url` in
`.claude-plugin/plugin.json`.

## What's in here

- **`skills/researching-vietnamese-law/`** — drives the agentic research loop
  (`deep_research` → `check_in_force` → `get_document` → `get_article` →
  `list_related`) and the Vietnamese legal tier reading order. `SKILL.md` holds the
  loop and judgment; `reference/` holds the tool JSON shapes (`tools.md`) and
  Vietnamese query craft (`query-craft.md`), loaded on demand.
- **`skills/checking-legal-coverage/`** — determines what the corpus covers by probing
  it live (coverage grows from the remote, so it's never asserted from memory). Answers
  "what can I ask?" / "is topic X in here?".
- **`commands/regai-doctor.md`** — `/regai-doctor` verifies the MCP connection and
  corpus reachability.

## The 6 tools (exposed over MCP)

| tool | purpose |
| --- | --- |
| `search` | hybrid (FTS + vector) search over articles |
| `get_article` | fetch one article's text |
| `get_document` | fetch a document's metadata + structure |
| `list_related` | follow the regulation chain (relations) |
| `check_in_force` | resolve a document's in-force status / successor |
| `deep_research` | deterministic multi-step research pipeline |

All `slug` / `doc` / `id` parameters accept **either** a vault slug (e.g.
`ND-155-2020`) **or** a số hiệu (e.g. `155/2020/NĐ-CP`).

## Example questions

- "Điều kiện chào bán chứng khoán ra công chúng là gì?"
- "Nghị định 58/2012 còn hiệu lực không?"
- "Which Nghị định / Thông tư implements Luật Chứng khoán 2019?"
- "What does the regai knowledge base cover?"
