# Assertline MCP server

[Assertline](https://assertline.com) is record-and-replay testing for web applications.
Its hosted MCP server lets an AI assistant find your recorded tests, run them and read
step-by-step results.

- **Address:** `https://app.assertline.com/api/mcp` (streamable HTTP)
- **Sign-in:** OAuth 2.1 with PKCE; the assistant registers itself, you sign in to
  Assertline in your browser and allow it. An API key in an `Authorization: Bearer` header
  also works.
- **Documentation:** https://assertline.com/docs#mcp
- This repository holds instructions and a Cursor plugin manifest only. The server is hosted; there is nothing to install or run.
- The files here are under the MIT licence. The Assertline service itself is not open source.

## Connect

**Claude (web, desktop):** Settings → Connectors → Add custom connector → paste the address.

**Claude Code**
```
claude mcp add --transport http assertline https://app.assertline.com/api/mcp
```
then `/mcp` and authenticate.

**Cursor** — `~/.cursor/mcp.json`
```json
{ "mcpServers": { "assertline": { "url": "https://app.assertline.com/api/mcp" } } }
```

**VS Code** — `.vscode/mcp.json`
```json
{ "servers": { "assertline": { "type": "http", "url": "https://app.assertline.com/api/mcp" } } }
```

**Cline** — in the Cline panel: MCP Servers → Configure → Remote Servers. Server Name
`assertline`, Server URL `https://app.assertline.com/api/mcp`, Transport Type
**Streamable HTTP**, then Add Server and sign in when Cline asks.

Or press **Configure MCP Servers** in the same tab and add the entry to the file it opens
(`cline_mcp_settings.json`; for current versions `~/.cline/data/settings/cline_mcp_settings.json`):
```json
{
  "mcpServers": {
    "assertline": {
      "type": "streamableHttp",
      "url": "https://app.assertline.com/api/mcp",
      "disabled": false,
      "autoApprove": []
    }
  }
}
```
`"type": "streamableHttp"` is needed: without it Cline tries the older SSE transport, which
this server does not offer. Cline does not read MCP servers from VS Code's `settings.json`.
Agents setting this up: see [llms-install.md](llms-install.md).

**Others:** add a remote (streamable HTTP) server with the address above.

## Tools

| Tool | What it does |
|---|---|
| `whoami` | Who you are connected as |
| `list_projects` | List projects |
| `list_tests`, `get_test` | The recorded tests of a project; one test with its steps |
| `run_test` | Start a run of a test |
| `run_batch` | Start a run of every test with a tag, optionally asking for an AI summary of each run (`aiAnalysis`) |
| `list_runs`, `get_run`, `get_run_results` | Runs, a run's status (for a finished run also its verdict, the steps that did not pass and its AI summary when one was written), the result of every step |
| `get_run_network` | The network problems of a run: calls that failed or were answered 4xx or 5xx |
| `list_batches`, `get_batch` | Batches and their runs, with each run's AI headline and cause when a summary was written |
| `list_environments`, `list_devices` | Where and at what screen size a test can run |
| `get_plan_and_usage` | The plan, its limits and this month's usage |

Starting a run answers at once with its id and a link; it does not wait for the result.
An assistant cannot create, change or delete tests, see or invite people, see keys, or pay.
Remove a connected assistant at any time in Assertline under Settings, Connected apps.

## You need
An Assertline account (free at https://app.assertline.com) with at least one recorded test.
To run a test from an assistant: the desktop app or command line signed in on a computer
(every plan, Free included), or the Pro or Enterprise plan for runs in Assertline's cloud.
AI summaries use your own OpenAI key, saved in Assertline under Settings. Plans and prices:
https://assertline.com/pricing
