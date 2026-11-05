# Connect Elaichi to Claude

Add `https://api.elaichi.ai/mcp` as a custom connector, then sign in with OAuth. This covers Claude on the web, Claude Desktop and Cowork. Steps checked October 2026 against Anthropic's help center. Anthropic's menus can change. The URL does not.

```text
https://api.elaichi.ai/mcp
```

## Plans

Custom connectors are offered on the Free, Pro, Max, Team and Enterprise plans. Free allows one custom connector. On Team and Enterprise, an Owner or Primary Owner adds custom connectors for the whole organization. On Enterprise, a member whose custom role includes Manage access to Libraries can also add one.

## Team and Enterprise: an Owner adds it once

1. Open **Organization settings**, then **Connectors**.
2. Select **Add**, hover over **Custom**, and choose **Web**.
3. Paste `https://api.elaichi.ai/mcp`.
4. Leave **Advanced settings** empty and select **Add**.

Then each member opens **Customize**, then **Connectors**, finds the Elaichi connector (usually marked Custom) and selects **Connect**. That sign-in is theirs alone.

## Pro and Max: add it yourself

1. Open **Customize**, then **Connectors**.
2. Select **+**, then **Add custom connector**.
3. Paste the URL, leave the advanced settings empty and select **Add**.

Anthropic documents these steps for Pro and Max. Free plans can add one custom connector.

## No client ID needed

Leave the OAuth client ID and secret empty. Elaichi supports dynamic client registration (RFC 7591) and publishes no client ID metadata document, so Claude registers itself.

## Turn it on in a chat

Under the **+** button in a chat, then **Connectors**, each connector has a toggle. The toggle turns the connector on or off for that conversation. It does not disconnect anything.

## Where Claude connects from

Claude reaches a custom connector from Anthropic's cloud, not from your device. Elaichi's endpoint is public, so there is nothing to allow on your network.

## Claude's tool permissions

Claude lets you set a connector's tools to Always allow, Needs approval or Blocked. Because Elaichi runs every connected tool through `execute_tool`, a permission on it covers every connected app at once. Keep per-tool decisions in Elaichi restrictions, which can tell a read from a delete.

Claude Code has its own page: [claude-code.md](claude-code.md).

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
