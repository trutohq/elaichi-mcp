# Connect Elaichi to Microsoft Copilot (through a Copilot Studio agent)

A maker adds Elaichi's endpoint to a Copilot Studio agent as an MCP tool, with OAuth 2.0 and Dynamic discovery, then publishes the agent to Microsoft Copilot. The Elaichi product team confirmed on October 21, 2026 that sign-in and connected tools work this way.

## Steps

1. In Copilot Studio, open the agent.
2. Go to **Tools**, then **Add a tool**, then **New tool**, then **Model Context Protocol**.
3. Enter the server URL:

```text
https://api.elaichi.ai/mcp
```

4. Choose **OAuth 2.0** with **Dynamic discovery**. Clients register themselves over OAuth, so there is no client ID or secret to enter.
5. Publish the agent to Microsoft Copilot.

## Whose account runs the call

Copilot Studio tools run on end-user credentials by default. Each person then signs in to Elaichi with their own account, and the calls run as them. Microsoft lets an admin forbid maker-provided credentials per environment.

## Where governance lives

Power Platform policies treat an MCP server as one unit. Microsoft states that per-tool MCP control is not available in advanced connector policies. A maker can switch off single tools inside one agent, but that is a build choice per agent. When the server is Elaichi, put the per-tool rule in Elaichi restrictions, written per role or per person.

## How Elaichi shows it

Elaichi does not mark Microsoft Copilot as a verified client. Copilot Studio uses a shared redirect host, so its audit log entries show the name the client registered for itself.

Related: [Microsoft Copilot with Salesforce: two paths](https://elaichi.ai/blog/microsoft-copilot-salesforce-jira/) and [Copilot Studio vs an MCP gateway](https://elaichi.ai/blog/copilot-studio-vs-mcp-gateway/).

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
