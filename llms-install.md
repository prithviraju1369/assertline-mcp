# Setting up the Assertline MCP server (for AI agents)

Assertline's MCP server is hosted at `https://app.assertline.com/api/mcp` (streamable
HTTP). Nothing is cloned, built, installed or started. Setting it up means adding one entry
to the MCP settings of the client you are running in, and the person signing in once.

## Cline

1. Find Cline's MCP settings file, `cline_mcp_settings.json`. It is the file opened by
   MCP Servers → Configure → Configure MCP Servers. Look in this order and use the first
   that exists:
   - the path in the `CLINE_MCP_SETTINGS_PATH` environment variable
   - `~/.cline/data/settings/cline_mcp_settings.json`
   - macOS: `~/Library/Application Support/Code/User/globalStorage/saoudrizwan.claude-dev/settings/cline_mcp_settings.json`
   - Windows: `%APPDATA%\Code\User\globalStorage\saoudrizwan.claude-dev\settings\cline_mcp_settings.json`
   - Linux: `~/.config/Code/User/globalStorage/saoudrizwan.claude-dev/settings/cline_mcp_settings.json`
2. Read the file. Add this entry under `mcpServers`, keeping every server already there:
   ```json
   "assertline": {
     "type": "streamableHttp",
     "url": "https://app.assertline.com/api/mcp",
     "disabled": false,
     "autoApprove": []
   }
   ```
3. Do NOT:
   - write to VS Code's `settings.json` (there is no `cline.mcpServers` setting; Cline ignores it)
   - leave out `"type": "streamableHttp"` (Cline would then try SSE, which fails)
   - add an `Authorization` header or ask the person for an API key; sign-in is OAuth
   - clone this repository or run any command
4. Tell the person: open MCP Servers in the Cline panel, find `assertline`, and press
   **Authenticate** if it is shown. A browser opens; they sign in to Assertline and allow
   the connection. An Assertline account is free at https://app.assertline.com.
5. Check it worked by calling the `whoami` tool of the `assertline` server. It answers with
   the organization the connection acts for. Only say the setup is finished after this call
   succeeds.

## Other clients

| Client | Where | Entry |
|---|---|---|
| Claude Code | run in a terminal | `claude mcp add --transport http assertline https://app.assertline.com/api/mcp` |
| Cursor | `~/.cursor/mcp.json` | `{ "mcpServers": { "assertline": { "url": "https://app.assertline.com/api/mcp" } } }` |
| VS Code (Copilot) | `.vscode/mcp.json` | `{ "servers": { "assertline": { "type": "http", "url": "https://app.assertline.com/api/mcp" } } }` |
| Claude (web, desktop) | Settings → Connectors → Add custom connector | paste the address |

## What the person needs
An Assertline account with at least one recorded test. To run a test from an assistant:
the Assertline desktop app or command line signed in on a computer, or a plan with cloud
runs. The tools are listed in [README.md](README.md).
