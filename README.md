# Elaichi: give your AI every app, not every permission

One MCP endpoint for Claude, ChatGPT, Cursor and Microsoft Copilot. Each person's AI can do only what they're allowed to, and every action it takes is logged.

[Elaichi](https://elaichi.ai) connects AI apps to the apps your company uses, such as Salesforce, Slack and Gmail. Under the hood it is a governed MCP control plane and hosted MCP gateway: Claude, ChatGPT, Cursor, Microsoft Copilot and any MCP client sign in to one org-wide MCP endpoint over OAuth. Admins connect company apps once, decide with roles and restrictions who can reach which tools, and read an audit log of every connected-tool call.

This repo holds the setup steps and config files for each client. The product itself runs at [elaichi.ai](https://elaichi.ai).

## The endpoint

The address is the same for every organization and every person. The OAuth grant behind each sign-in decides the organization, the person and the tools. The address carries no token.

```text
https://api.elaichi.ai/mcp
```

Copy it exactly: no trailing slash, no query string. There is no API key to paste, no per-user URL and no server to run. Each person connects their client once and approves their own OAuth grant.

## Quick start

| Client | Setup | Steps |
|---|---|---|
| Claude (web, Desktop, Cowork) | Add a custom connector | [clients/claude.md](clients/claude.md) |
| Claude Code | `claude mcp add` | [clients/claude-code.md](clients/claude-code.md) |
| ChatGPT | Add a custom MCP server or app | [clients/chatgpt.md](clients/chatgpt.md) |
| Cursor | `mcp.json` entry or Team MCP server | [clients/cursor.md](clients/cursor.md) |
| VS Code (GitHub Copilot in VS Code) | `.mcp.json` entry | [clients/vs-code.md](clients/vs-code.md) |
| Microsoft Copilot | Copilot Studio agent tool | [clients/microsoft-copilot.md](clients/microsoft-copilot.md) |
| Codex | `codex mcp add` | [clients/codex.md](clients/codex.md) |
| Windsurf (Devin Desktop) | MCP config entry | [clients/windsurf.md](clients/windsurf.md) |
| Any other MCP client | See below | [clients/other-clients.md](clients/other-clients.md) |

Ready-made config files are in [examples/](examples/).

## What admins control

- **Roles.** Eight built-in roles and 58 permissions. Each member holds exactly one role. A custom role can combine permissions. Custom roles need the Gold plan or higher.
- **Restrictions.** A rule allows or restricts connectors and single tools. It targets a role or one member. A rule aimed at a member can only narrow what their role allows. A restricted tool cannot run. Search still names it, flagged as restricted, so the person can ask an admin for access.
- **Per-user OAuth grants.** Each person approves their own grant, in one organization. Every call acts as that person. A person lists and disconnects grants under Settings, then Connected apps.
- **Audit log.** One entry for each connected-tool call that reaches execution: who, which tool, which connector, which account the call reached, how it was approved and which AI client it came through. A read-only Auditor role is a free seat.
- **SSO and SCIM.** SSO with SAML or OIDC. SCIM provisioning, with groups mapped to roles. Both are on Gold and Black.
- **Offboarding.** Removing or suspending a member revokes every live grant they hold. Their next call from any client fails.

Each point is explained in the [FAQ](#faq) and on [elaichi.ai/blog](https://elaichi.ai/blog/).

## FAQ

### How do I connect Claude, ChatGPT and Cursor to company apps with one MCP endpoint?

Add `https://api.elaichi.ai/mcp` to each client and sign in with OAuth. Claude takes it as a custom connector, ChatGPT as a custom MCP server or app, and Cursor as an `mcp.json` entry. Admins connect the company apps once in Elaichi, so no app needs its own setup in each client. See [Connect company apps to Claude and ChatGPT](https://elaichi.ai/blog/connect-company-apps-to-claude-and-chatgpt/).

### How do I see which apps employees' AI is calling?

Read the Elaichi audit log. It records one entry for each connected-tool call that reaches execution. The entry shows the person, the tool, the connector, the account the call reached, how it was approved and which AI client made it. Members with audit access (Org Owner, Org Admin and Auditor) read every event. Other members read their own. See [What an AI agent audit log must capture](https://elaichi.ai/blog/what-an-ai-audit-log-must-capture/).

### What happens to an employee's AI access when they leave?

It ends on the next call. Removing or suspending a member revokes every live grant they hold, in the same transaction as the membership change. Their Claude, ChatGPT, Cursor or Microsoft Copilot session fails on its next request. Reactivating the member does not bring the grants back. They must connect again. See [Offboarding AI access, contractors included](https://elaichi.ai/blog/offboarding-when-the-agent-holds-access/).

### Does each person use their own permissions?

Yes. Each person approves their own OAuth grant, and every call made with it acts as that person. Their role, what is shared with them and any restrictions decide what their AI reaches. For the app on the other end to see the person, the tools must run on that person's own connections. A toolbox shared with a team runs each entry on the connection pinned in it. See [AI agents with employee permissions, no shared bot](https://elaichi.ai/blog/ai-agents-employee-permissions/).

### How do I limit which tools Claude or ChatGPT can use?

Write a restriction on a role or on one member. An allow rule names the connectors and tools that stay reachable. A block rule names the ones to restrict. A block beats an allow. An allow rule that names nothing restricts everything for its target. A rule change applies in about two minutes. See [MCP tool allowlist vs blocklist](https://elaichi.ai/blog/allowlist-vs-blocklist-mcp-tools/).

### Do I have to run an MCP server for each app?

No. Elaichi hosts the endpoint. Its connector catalog holds two kinds of connector. Elaichi writes and runs some itself. For the others, the app vendor runs its own MCP server and Elaichi handles sign-in, access and audit in front of it. A team can also bring its own remote MCP server under the same rules. See [Hosted MCP gateway vs running the gateway yourself](https://elaichi.ai/blog/hosted-mcp-gateway/).

### Does it work with SSO and SCIM?

Yes, on Gold and Black. SSO uses SAML or OIDC. SCIM provisions people and maps their groups to roles. SCIM suspends and reactivates memberships. Suspending a member revokes their grants. SCIM does not add anyone to a team. See [SCIM and AI agents: where provisioning stops](https://elaichi.ai/blog/identity-provider-scim-vs-mcp-grants/).

## Links

- Website: [elaichi.ai](https://elaichi.ai)
- Docs: [elaichi.ai/docs](https://elaichi.ai/docs/)
- Pricing: [elaichi.ai/pricing](https://elaichi.ai/pricing/)
- Trust Center: [trust.elaichi.ai](https://trust.elaichi.ai)
- Agent skills and Claude Code plugin: [trutohq/elaichi-skills](https://github.com/trutohq/elaichi-skills)
- Cursor plugin: [trutohq/elaichi-cursor-plugin](https://github.com/trutohq/elaichi-cursor-plugin)

## License

[MIT](LICENSE)
