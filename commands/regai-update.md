---
description: Download or refresh the Vietnamese legal corpus from the remote.
---

Refresh the regai legal corpus to the latest published version.

Run the bundled regai binary's `update` subcommand. Pick the binary for the
current OS/arch from `${CLAUDE_PLUGIN_ROOT}/bin/`:

- macOS Apple Silicon: `${CLAUDE_PLUGIN_ROOT}/bin/regai-macos-arm64`
- Windows x64: `${CLAUDE_PLUGIN_ROOT}/bin/regai-windows-x64.exe`

Steps:

1. Detect the platform (use `uname -sm` on Unix; on Windows the `.exe` applies).
2. Run: `<binary> update`
3. Report the JSON result to the user in plain language:
   - `"status": "updated"` → "Corpus updated to <version> (<corpus_note>)."
   - `"status": "up_to_date"` → "Corpus already current (<version>)."
   - `"status": "error"` → relay the `detail` and suggest retrying.

The corpus is stored at `~/.regai/sec.db`; the binary reads it by default.
