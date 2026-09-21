---
description: Check the regai MCP connection and verify the corpus is reachable.
---

Verify the regai MCP service is connected and answering.

1. Check whether the `regai` MCP tools are available in this session (e.g.
   `check_in_force`, `search`). If they are NOT present, the server is not
   connected. Suggest the user continue with the **regai-vault-search** skill
   (bundled vault snapshot, offline). Also report:
   - The plugin declares a remote MCP server at
     `https://psvpergdfbqzsagaacpz.supabase.co/functions/v1/mcp`. Confirm the
     plugin is enabled and the server was approved (per-server approval prompt
     at enable time).
   - The hosted server is a public Supabase Edge Function. The plugin connects
     anonymously.

2. If the tools ARE available, call `check_in_force` with a known document
   (`slug: "54/2019/QH14"`). Expected: a JSON object with `tinh_trang` and
   `in_force_slug`. Report success and the resolved status.

3. If the call errors with a connection error, surface it verbatim, confirm
   the service URL `https://psvpergdfbqzsagaacpz.supabase.co/functions/v1/mcp`
   is reachable, and suggest the user continue with **regai-vault-search**.
