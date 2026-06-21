# regai — Vietnamese Legal Research, in Plain Language

**Product onepager · v0.6.0 · Infina Legal Tools**

---

## The problem

Researching Vietnamese law is slow and error-prone. The law is layered (Luật →
Nghị định → Thông tư), documents get superseded silently, and a correct answer
depends on reading the *right* article in a document that is *still in force* and
citing it precisely. General-purpose AI makes this worse: it confidently invents
statutes, articles, and citations that don't exist — a non-starter for anything
legal.

## What regai is

**regai is a Claude Code plugin for researching Vietnamese law in plain
language.** Ask a question in Vietnamese or English; Claude drives a
deterministic, **LLM-free retrieval engine** over a curated legal corpus and
answers with **citable sources**.

The distinction that matters: *the retrieval engine never uses an LLM, so it
cannot hallucinate law.* Claude only synthesizes an answer from the actual legal
text the tools return. Nothing decides the law for you — the tools return legal
data, Claude arranges it.

## Why it's different

| | General AI chat | Generic legal search | **regai** |
| --- | --- | --- | --- |
| Hallucination risk | High | — | **None — LLM-free retrieval** |
| Plain-language Q&A | Yes | No | **Yes (VN + EN)** |
| Cites exact article + status | No | Partial | **Yes (số hiệu + Điều + in-force)** |
| Respects legal hierarchy | No | No | **Yes (Luật → Nghị định → Thông tư)** |
| Checks if law is still valid | No | Rarely | **Yes (`check_in_force`)** |

## How it works

regai connects to a **centrally hosted MCP service** — nothing to download,
build, or keep updated. The corpus (securities law and connected documents) is
maintained remotely. The plugin exposes **6 tools** to Claude:

| tool | purpose |
| --- | --- |
| `search` | hybrid full-text + vector search over articles |
| `deep_research` | deterministic multi-step pipeline returning the full regulatory stack |
| `get_article` | fetch one article's exact text before quoting it |
| `get_document` | fetch a document's metadata + structure |
| `list_related` | follow the regulation chain (e.g. which Nghị định implements a Luật) |
| `check_in_force` | resolve a document's validity / find its successor |

A built-in research skill enforces the discipline a careful lawyer would:
read the article body before quoting, check `check_in_force` before citing, and
always cite with **số hiệu + Điều number + in-force status**. Every parameter
accepts either a vault slug (`ND-155-2020`) or a legislative number
(`155/2020/NĐ-CP`).

## Who it's for

- **Lawyers, in-house counsel, and paralegals** working with Vietnamese law
- **Compliance teams** tracking securities regulation and rule changes
- **Business & ops teams** launching in Vietnam who need conditions, procedures, and thresholds
- **Analysts and researchers** studying Vietnamese regulation

## What it looks like in use

- *"Điều kiện chào bán chứng khoán ra công chúng là gì?"* — conditions for a public securities offering, cited to the controlling Nghị định.
- *"Nghị định 58/2012 còn hiệu lực không?"* — validity check, with the successor document if superseded.
- *"Which Nghị định / Thông tư implements Luật Chứng khoán 2019?"* — the full implementing stack.
- *"What does the regai knowledge base cover?"* — coverage probed live, never guessed.

## Getting started

```
/plugin marketplace add infina-pm/regai-plugin
/plugin install regai@regai
```

On enable, approve the remote MCP server and (optionally) enter an API token —
regai works anonymously; a token just adds per-user attribution. Then verify
with `/regai-doctor` or simply ask *"check the regai connection."*

---

*Built on the Model Context Protocol · Hosted, curated corpus, continuously
updated · github.com/infina-pm/regai-plugin*
