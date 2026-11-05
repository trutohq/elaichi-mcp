# Connect Elaichi to Claude Code

Run one command with the endpoint, then sign in from the browser. Checked October 2026 against Anthropic's Claude Code docs.

```bash
claude mcp add --transport http elaichi --scope user https://api.elaichi.ai/mcp
```

Do not add `--header`. Elaichi takes no pasted token.

## Scopes

- **User scope** (above) loads in all your projects. It is stored in `~/.claude.json`.
- **Local scope** is the default and loads in one project.
- **Project scope** is shared through version control in `.mcp.json` at the repository root. The file holds no secret, so it is safe to commit. See [examples/claude-code-mcp.json](../examples/claude-code-mcp.json).

In interactive sessions, Claude Code asks each engineer to approve a project server before it connects. Each engineer still signs in as themselves.

## Sign in

Start sign-in with `/mcp` inside a session, or run `claude mcp login elaichi` from the shell. Over SSH, `claude mcp login` prints the URL. `--no-browser` forces that. Claude Code registers itself through dynamic client registration, so there is no client ID to enter.

## Check it

- `claude mcp list` shows each server with a status such as Connected or Needs authentication.
- `claude mcp get elaichi` shows one server.
- Ask a real question, such as "List my open Jira issues".

## Use the Claude connector instead

Anthropic's docs say connectors added in claude.ai are available in Claude Code when you log in with a claude.ai account. If Elaichi is added as a custom connector in Claude, it can appear in `/mcp` with no command. This works with a claude.ai subscription login, not with an API key. A server you add yourself with the same URL takes precedence.

## Disconnect

Run `claude mcp logout elaichi` to clear the credentials Claude Code stores. To end the grant in Elaichi, disconnect it under **Settings**, then **Connected apps**.

## How Elaichi shows Claude Code

Claude Code registers only loopback redirect addresses, so Elaichi never shows it as a verified client. Its entries in the audit log carry the name Claude Code registered for itself.

Claude on the web and Desktop has its own page: [claude.md](claude.md).

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
