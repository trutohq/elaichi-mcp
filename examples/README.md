# Examples

Config files for clients that read a file. Each one holds only the public endpoint. There is no key, no header and no client ID. Each person signs in with OAuth.

| File | Put it at | Client |
|---|---|---|
| `cursor-mcp.json` | `~/.cursor/mcp.json` or `.cursor/mcp.json` | [Cursor](../clients/cursor.md) |
| `vscode-mcp.json` | `.mcp.json` at the workspace root | [VS Code](../clients/vs-code.md) |
| `claude-code-mcp.json` | `.mcp.json` at the project root | [Claude Code](../clients/claude-code.md) |
| `windsurf-mcp_config.json` | `~/.config/devin/mcp_config.json` | [Windsurf (Devin Desktop)](../clients/windsurf.md) |
| `codex-config.toml` | `~/.codex/config.toml` | [Codex](../clients/codex.md) |

Add the entry next to any servers already in the file. Claude, ChatGPT and Microsoft Copilot take the URL in their own settings, not in a file. See their pages under [clients](../clients/).
