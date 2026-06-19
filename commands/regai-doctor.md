---
description: Check the regai MCP connection and verify the corpus is reachable.
---

Verify the regai MCP service is connected and answering.

1. Check whether the `regai` MCP tools are available in this session (e.g.
   `check_in_force`, `search`). If they are NOT present, the server is not
   connected — report:
   - The plugin declares a remote MCP server. Confirm the plugin is enabled and the
     server was approved (per-server approval prompt at enable time).
   - Confirm the user entered a valid token in the plugin's `api_token` config. A
     401 means the token is missing or wrong — re-enter it in plugin settings.

2. If the tools ARE available, call `check_in_force` with a known document
   (`slug: "54/2019/QH14"`). Expected: a JSON object with `tinh_trang` and
   `in_force_slug`. Report success and the resolved status.

3. If the call errors with an auth/connection error, surface it verbatim and point
   the user to re-check their token and that the service URL is reachable.
