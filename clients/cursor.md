# Connect Elaichi to Cursor

Add `https://api.elaichi.ai/mcp` as a remote MCP server with a `url` field, then authenticate. Menu names are from Cursor 3.24, checked live on October 16, 2026.

## Each developer: mcp.json

1. In Cursor's Agents window, open **Customize**, then **MCPs**, and select **New MCP Server**. That opens `~/.cursor/mcp.json`.
2. Add this entry next to any servers already in the file, and save.

```json
{
  "mcpServers": {
    "elaichi": {
      "url": "https://api.elaichi.ai/mcp"
    }
  }
}
```

3. Elaichi is listed as Needs authentication. Select **Authenticate**. Elaichi's sign-in opens in the browser and names Cursor as the requesting app.
4. After **Allow access**, the browser shows an Authorization complete page at a localhost address and asks to open Cursor. Cursor then lists Elaichi as connected, with its tools enabled.

Cursor reads servers from `~/.cursor/mcp.json` (every project) and from `.cursor/mcp.json` in a project. A project file is safe to commit. It holds a public URL and no credential. See [examples/cursor-mcp.json](../examples/cursor-mcp.json).

Use no `headers` block and no `env` key. Cursor supports OAuth and registers itself with Elaichi, so there is no client ID or secret to paste.

## Team admins: a shared Team MCP server

Team admins can distribute shared MCP servers. Each developer still installs it and signs in.

1. Under **Dashboard**, then **Plugins & MCPs**, configure Elaichi as a Team MCP server with the URL above.
2. Under **Team MCP Servers**, select **Add to Team Marketplace**. That makes it available in Cursor's Agent Window, IDE and CLI.
3. Ask developers to install it from **Customize** and sign in.

A marketplace listing is an offer. It does not install or enable the server for anyone.

## Enterprise allowlist

Cursor's MCP allowlist is an Enterprise feature, set in the team dashboard under **MCP Configuration**. If an allowlist is active, add the Elaichi URL or Cursor will not run the server. An allowlist approves a server but does not push it to users' machines.

## Approvals

Cursor asks for approval before using MCP tools by default. For an Elaichi call the prompt names `execute_tool`, and the real tool is in the arguments. Expand them before you approve a write. Cursor's allowlist rules cannot tell a read from a delete, because every connected tool runs through `execute_tool`. Write that distinction in Elaichi restrictions.

## What the consent screen asks

Elaichi opens its own sign-in and consent screen in the browser.

1. Pick one organization. With one membership it is preselected. A grant covers exactly one organization.
2. Tick the permissions. Up to four boxes appear: **Read your organization's data**, **Create and change data**, **Run your connected tools** and **Delete data and remove access**. Everything the client asked for starts ticked except delete, which is never pre-ticked.
3. Keep **Run your connected tools** ticked, or the client reaches no connected app. A second step then asks which toolboxes the client may use: **All my tools** (the default) or **Only the ones I pick**.

A consent request lasts 30 minutes. A scope cannot be added to a grant later. To add one, connect again and tick it.

## If it does not work

- Check the URL first. It is `https://api.elaichi.ai/mcp`, on the `api` host, with no trailing slash. With a trailing slash the endpoint returns 404. On `app.elaichi.ai` it returns 405. Neither starts sign-in.
- The tool list is short on purpose. Elaichi lists its search and execute tools. It never lists connected tools one by one. The client finds a tool with `search_tools` and runs it with `execute_tool`.
- An empty list for a whole role can mean the role lacks the `tool:execute` permission. An admin fixes that.
- The endpoint allows 120 requests per minute for each token. Over that, wait a minute.
- Sign-in needs a browser. A client running headless in CI cannot finish it.

More: [Fixing OAuth errors when adding an MCP connector](https://elaichi.ai/blog/mcp-oauth-errors/) and [MCP tools not showing up](https://elaichi.ai/blog/mcp-tools-not-showing/).
