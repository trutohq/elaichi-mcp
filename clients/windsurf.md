# Connect Elaichi to Windsurf (Devin Desktop)

Windsurf is now named Devin Desktop (checked October 2026). Add one entry with `url` to its MCP config file, then sign in.

## Config file

Both of its agents read `~/.config/devin/mcp_config.json` (`%APPDATA%\devin\mcp_config.json` on Windows). This entry fits both:

```json
{
  "mcpServers": {
    "elaichi": {
      "url": "https://api.elaichi.ai/mcp"
    }
  }
}
```

See [examples/windsurf-mcp_config.json](../examples/windsurf-mcp_config.json).

- In Cascade, open the file from the **...** menu at the top right of the Cascade panel, then **Open MCP config file**.
- For Devin Local, a project's `.devin/mcp_config.json` is shared through version control.
- With Devin CLI installed, `devin mcp add -s user elaichi https://api.elaichi.ai/mcp` writes the user entry for you.

## Sign in

An OAuth server prompts for sign-in the first time it is used. `devin mcp login elaichi` starts it directly. Devin registers itself through dynamic client registration, so there is no client ID to enter. Each editor signs in on its own.

## Check it

Cascade's **...** menu has an MCPs section that shows each server and its enabled tool count. With Devin CLI, `devin mcp list` shows configured servers. When stored sign-in credentials expire, Devin Local shows **Needs auth**, and **Authenticate** runs the browser flow again.

## Allowlist

Once a team admin allowlists one server, every server not on the list cannot run. The allowlist's Server ID must match the entry's key, so name the entry `elaichi` in every file.

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
