# regai — your in-house Vietnamese legal consultant (Claude Code plugin)

Do Vietnamese legal work in plain language, with answers you can actually rely on.
A general AI assistant answers from whatever it half-remembers from training — often
outdated, usually unsourced, sometimes simply wrong. regai is built differently:

- **Right, and current.** Every answer is grounded in a corpus built specifically for
  Vietnamese law and kept up to date — not a model's training data. regai enforces
  what a careful lawyer checks: whether a document is still in force, what superseded
  it, and which related Nghị định / Thông tư apply. It reads the law as it stands
  today and cites it (số hiệu + Điều + tình trạng).
- **Comes with the playbook.** Built-in workflows and templates for real work —
  drafting, contract and NDA review, legal memos, compliance mapping — so it performs
  like an experienced associate from the first request, not a blank chatbot.
- **Becomes yours.** Tailor it to your industry and house style. It compounds what it
  learns across matters into a local `.regai/` store you own, growing into a real
  in-house consultant that remembers your context, parties, and standing preferences.

The corpus (Vietnamese securities law and the documents it connects to, and growing)
is hosted centrally and kept current — there's nothing to download or build. regai's
grounding engine is a standard **MCP** service, so it works in any MCP-capable
assistant — Claude (Code or cowork), Codex, editors, your own agent. This plugin is
the turnkey Claude Code packaging; to use regai elsewhere, point your MCP client at
the hosted server (see [Use it in other MCP clients](#use-it-in-other-mcp-clients)).

## Install

This repo is a single-plugin Claude Code marketplace.

```
/plugin marketplace add infina-pm/regai-plugin
/plugin install regai@regai
```

(Or `/plugin marketplace add <path-to-this-repo>` for a local clone.)

On enable, Claude Code prompts you to:

1. **Approve the remote MCP server** (`regai` over HTTP).
2. **Enter your `api_token`** — optional. Leave blank to use regai anonymously; provide
   a token (ask the regai maintainer) for per-user attribution. Run `/regai-setup` and
   Claude writes the token into your settings for you.

Then verify with `/regai-doctor` or just ask: *"check the regai connection."*

The hosted service is `https://regai-dn2n.onrender.com` (baked into the plugin). To
point at a local dev server instead, edit `mcpServers.url` in
`.claude-plugin/plugin.json`.

## Use it in other MCP clients

This plugin bundles the skills and `/` commands for Claude Code, but the tools
themselves are plain MCP — usable from any MCP client (Claude cowork, Codex, editors,
your own agent). Point the client at:

- **URL:** `https://regai-dn2n.onrender.com/mcp` (HTTP / streamable MCP)
- **Auth:** `Authorization: Bearer <api_token>` — optional; omit the header to use
  regai anonymously.

You get the same six grounded tools. The packaged skills and playbooks are Claude
Code-specific for now; in other clients you drive the tools directly, or prompt your
agent through the same ask → check → ship-a-goal loop.

## What's in here

- **`skills/regai-checking-current-law/`** — answers point questions by driving the
  agentic research loop (`deep_research` → `check_in_force` → `get_document` →
  `get_article` → `list_related`) in Vietnamese legal tier order. `reference/` holds
  the tool JSON shapes (`tools.md`) and query craft (`query-craft.md`).
- **`skills/regai-listing-coverage/`** — determines what the corpus covers by probing
  it live (coverage grows from the remote, never asserted from memory). Answers "what
  can I ask?" / "is topic X in here?".
- **`skills/regai-shipping-goal/`** — the goal-driven legal associate. Plan → approve →
  execute with checkpoints → deliver → learn. Ships team-baseline `playbooks/` and
  `templates/`; overlays a per-workspace `.regai/` store for your org context and
  compounding `memory/learnings.md`.
- **`skills/regai-setup/`** — `/regai-setup` writes your optional `api_token` into
  Claude's settings (regai works tokenless, so this is only for per-user attribution).
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

## Try it

Ask:
- "Điều kiện chào bán chứng khoán ra công chúng là gì?"
- "Nghị định 58/2012 còn hiệu lực không?"
- "What does the regai knowledge base cover?"

Ship a goal:
- "Review this NDA for me."
- "Draft a mutual NDA between [A] and [B]."
- "Write a memo on the disclosure obligations for a public offering."
- "Map the compliance obligations for [activity]."
