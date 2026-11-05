# Connect Elaichi to Codex

Add the endpoint as a streamable HTTP server, then run `codex mcp login elaichi`. Checked October 2026 against OpenAI's Codex docs.

```bash
codex mcp add elaichi --url https://api.elaichi.ai/mcp
codex mcp login elaichi
```

Leave out `--bearer-token-env-var` and `--oauth-client-id`. Elaichi takes no pasted token and needs no client ID.

## config.toml

Codex keeps MCP servers in `~/.codex/config.toml`. A project can scope one in `.codex/config.toml`, in trusted projects only.

```toml
[mcp_servers.elaichi]
url = "https://api.elaichi.ai/mcp"
```

See [examples/codex-config.toml](../examples/codex-config.toml). OpenAI's docs say the ChatGPT desktop app, the Codex CLI and the IDE extension share this configuration, so adding Elaichi once covers all three.

In the IDE extension, open the gear menu, then **MCP servers**, then **Add server**. Enter `elaichi`, choose **Streamable HTTP**, paste the URL, save and select **Restart extension**. **Authenticate** starts the sign-in.

## Check it

`codex mcp list` shows configured servers. `codex mcp get elaichi` shows one. Inside the `codex` TUI, `/mcp` shows the active servers.

## Approvals

Codex sets approvals per server with `default_tools_approval_mode`, which takes `auto`, `prompt`, `writes` or `approve`. Every connected Elaichi tool arrives through `execute_tool`, so Codex's `enabled_tools` and `disabled_tools` lists cannot tell a read from a delete. Elaichi restrictions can.

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
