# Connect Elaichi to ChatGPT

Add `https://api.elaichi.ai/mcp` as a custom MCP server, keep Authentication on OAuth, and sign in. Menu paths checked October 2026. OpenAI moves its menus, so dates matter. The URL does not change.

```text
https://api.elaichi.ai/mcp
```

## Plans

OpenAI says custom MCP server connections and full MCP support are available for ChatGPT Business and Enterprise/Edu on ChatGPT web. ChatGPT Pro can connect custom MCP servers with read and fetch permissions. OpenAI's article names no way to add a custom MCP server on Free or Plus.

## Pro: add it yourself

Checked live on ChatGPT Pro, October 15, 2026.

1. In ChatGPT, open **Plugins** from the left bar. That opens Customize.
2. Select **Add**, then **Add custom MCP server**.
3. Name it Elaichi, paste `https://api.elaichi.ai/mcp` and leave **Authentication** on OAuth. The form asks for no client ID or secret.
4. Tick **I understand and want to continue**. **Create as a plugin** stays off until you do. Then select it.
5. Select **Continue to Elaichi**, sign in and choose what ChatGPT may do.

Elaichi is then listed under Plugins, then Personal, with a **Try in chat** button. The plugin's settings page offers **Refresh tools** and a **Permission** choice: Always ask, Allow read-only tools, Allow low-risk tools by default or Allow all tools.

## Business: an admin creates the app once

On Business, only admins and owners can create and publish a custom MCP app for the workspace. On Enterprise and Edu, admins can use role-based access control to give chosen members the Create plugins with MCPs permission. Only admins and owners publish.

1. Open **Workspace settings**, then **Apps**, then **Create**.
2. Enter `https://api.elaichi.ai/mcp` and choose OAuth as the authentication.
3. Select **Scan Tools** and complete the OAuth prompt so the scan can finish.
4. Select **Create**. The app is saved as a draft.
5. Publish it from **Workspace settings**, then **Apps**, then **Drafts**, then **Publish**. Published apps appear in users' Apps settings with the label custom.

OpenAI's page says Business apps cannot be updated after publishing. Check the name and settings before you publish.

## Write actions

ChatGPT may ask for confirmation before a write, depending on the app's permissions. Do not rely on that alone. A tool that an Elaichi restriction withholds cannot be called, however the prompt is phrased.

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
