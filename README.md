# Assertline MCP server

[Assertline](https://assertline.com) is record-and-replay testing for web applications.
Its hosted MCP server lets an AI assistant find your recorded tests, run them and read
step-by-step results.

- **Address:** `https://app.assertline.com/api/mcp` (streamable HTTP)
- **Sign-in:** OAuth 2.1 with PKCE; the assistant registers itself, you sign in to
  Assertline in your browser and allow it. An API key in an `Authorization: Bearer` header
  also works.
- **Documentation:** https://assertline.com/docs#mcp
- This repository holds instructions only. The server is hosted; there is nothing to install or run.

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

**Cline and others:** add a remote (streamable HTTP) server with the address above.

## Tools

| Tool | What it does |
|---|---|
| `whoami` | Who you are connected as |
| `list_projects` | List projects |
| `list_tests`, `get_test` | The recorded tests of a project; one test with its steps |
| `run_test` | Start a run of a test |
| `run_batch` | Start a run of every test with a tag |
| `list_runs`, `get_run`, `get_run_results` | Runs, a run's status, the result of every step |
| `list_batches`, `get_batch` | Batches and their runs |
| `list_environments`, `list_devices` | Where and at what screen size a test can run |
| `get_plan_and_usage` | The plan, its limits and this month's usage |

An assistant cannot create, change or delete tests, see or invite people, see keys, or pay.
Remove a connected assistant at any time in Assertline under Settings, Connected apps.

## You need
An Assertline account (free at https://app.assertline.com) with at least one recorded test.
To run a test from an assistant: the desktop app or command line signed in on a computer,
or a plan with cloud runs.
