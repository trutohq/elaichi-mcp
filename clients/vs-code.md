# Connect Elaichi to VS Code (GitHub Copilot in VS Code)

Add one entry with `type` and `url`, then sign in. The Elaichi product team confirmed on October 20, 2026 that GitHub Copilot in VS Code signs in to Elaichi over OAuth and runs connected tools through the MCP endpoint. VS Code steps checked October 2026.

## Add the server

Run **MCP: Add Server** from the Command Palette, or edit a file. The guided flow offers `.mcp.json` at the workspace root, or Copilot Global (`~/.copilot/mcp-config.json` by default). VS Code's docs say to prefer those portable destinations for new servers.

```json
{
  "mcpServers": {
    "elaichi": {
      "type": "http",
      "url": "https://api.elaichi.ai/mcp"
    }
  }
}
```

See [examples/vscode-mcp.json](../examples/vscode-mcp.json). The older `.vscode/mcp.json` uses a top-level `servers` key instead of `mcpServers`.

Leave out `headers`. Leave out the `oauth` block too. That block supplies a client ID, and Elaichi needs none. A workspace `.mcp.json` follows Workspace Trust, so the server starts once you trust the repository.

## Check it

Select **Configure Tools** in the chat input. If the server fails to start, run **MCP: List Servers**, pick it and choose **Show Output**.

VS Code allows at most 128 tools enabled in a chat request. Connected apps add nothing to that count, because Elaichi lists only its search and execute tools.

## How Elaichi shows it

Elaichi does not mark VS Code as a verified client. Its entries in the audit log show the name VS Code registered for itself, flagged as provided by the client.

## Editor policy

The `ChatMCP` policy can limit VS Code to a curated MCP registry or turn MCP off. Copilot Business and Enterprise organizations can also set MCP access in GitHub organization settings. Editor policy decides which servers may run. Elaichi restrictions decide which connectors and tools a person may reach.

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
