# regai privacy policy

Last updated: 2026-10-05

The regai plugin connects Claude to the hosted regai MCP server
(`https://psvpergdfbqzsagaacpz.supabase.co/functions/v1/mcp`), operated by Infina.
This page describes what that server receives and keeps.

## What we collect

Each tool call (`search`, `get_article`, `get_document`, `list_related`,
`check_in_force`, `deep_research`) is logged with:

- the tool name and the arguments Claude sent (for example, the search query text),
- the IDs of the documents returned (not their text) and the result count,
- whether the call failed, plus the error message,
- the response time and a timestamp,
- an anonymous user ID tied to your access token, or a shared anonymous ID if you
  use the plugin without a token.

We do not collect your name, email, or your conversation with Claude, and the query
logs do not store your IP address. Our hosting provider (Supabase) keeps standard
short-lived request logs for operating its platform.
Only the arguments of tool calls reach the server, so do not put personal or
confidential details in a legal question you want looked up.

## Local files

The skills may write a profile of your organisation and house style to
`.regai/` in your own workspace. Those files stay on your machine and are never
sent to the server.

## How we use it

Logs are used only to operate, debug and improve regai (for example, to find
questions the corpus cannot answer). We do not sell them or share them with
third parties, other than our hosting provider (Supabase), which stores them.

## Retention and deletion

Logs are kept for as long as they are useful to improve the service. To have the
logs for your token deleted, open an issue at
https://github.com/infina-pm/regai-plugin/issues with your anonymous user ID.

## Changes

Updates to this policy are published in this file.
