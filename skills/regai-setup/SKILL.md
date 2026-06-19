---
name: regai-setup
description: Use when a user wants to set up or fix their regai API token — "set up regai", "connect regai", "my regai token", "regai says unauthorized", or runs /regai-setup. Writes the token into Claude Code settings for them.
---

# regai setup

Goal: get the user's personal regai token into Claude Code's settings so the
regai MCP tools authenticate. The user is likely non-technical — do the file
edit for them; do not ask them to edit JSON by hand.

## Step 1 — Find the settings file and the regai plugin entry

Read `~/.claude/settings.json` (expand `~` to the user's home). If it does not
exist, you will create it. Locate the regai plugin's config under
`pluginConfigs` — scan the keys for the one whose id contains `regai`. Remember
that exact key (do NOT hardcode a guess; different installs use different ids).

## Step 2 — Check whether a token is already set

Look at `pluginConfigs.<regai-key>.options.api_token`.

- **If it is empty or missing:** go to Step 3 (set it).
- **If it is already set:** go to Step 5 (verify) — the user may have just
  reconnected, or wants to confirm it works.

## Step 3 — Ask for the token and write it

Ask the user to paste the regai token the maintainer gave them (a UUID like
`11111111-2222-3333-4444-555555555555`).

Then update `~/.claude/settings.json`, preserving every other key:
- Ensure `pluginConfigs` exists, ensure the regai entry exists, ensure its
  `options` object exists.
- Set `options.api_token` to the pasted value.
- Write the file back as valid JSON (same indentation as the original).

Use the file-editing tools for this — read the whole file, merge in memory,
write it back. Never delete or reorder unrelated keys.

## Step 4 — Tell the user the one manual step

The MCP connection only reads the token when a session starts, so tell the user:

> Saved. To finish, **open a new session** — in the CLI start a fresh `claude`
> session; in the desktop app open a new window (or restart it). Then ask me a
> Vietnamese-law question and I'll confirm it's working.
> (Power users: `/mcp` → regai → reconnect also works.)

Stop here — you cannot verify in this session, because this session is still on
the old connection.

## Step 5 — Verify (only when a token is already configured)

Call the regai `search` tool with a trivial query (e.g. `chào bán chứng khoán`).

- **Hits returned:** tell the user regai is connected and ready.
- **Unauthorized / 401 / connection error:** the token is wrong or the session
  has not picked it up yet. Re-confirm the token with the user (re-run Step 3 if
  it is wrong), then have them open a new session and try again.
