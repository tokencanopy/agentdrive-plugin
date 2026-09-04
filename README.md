# AgentDrive Plugin & Connector

AgentDrive (by Mnexa, Inc., in private preview) is an artifact store and remote drive for AI agents — this repository hosts the AgentDrive agent plugin, marketplace manifests, and MCP connector metadata, not the client SDKs.

## MCP Endpoint

```
https://drive.mcp.tokencanopy.com/mcp
```

Streamable HTTP, OAuth 2.1 PKCE, no API key required.

## Quick Install

### Claude Code

```bash
claude plugin marketplace add tokencanopy/agentdrive-plugin
claude plugin install agentdrive@agentdrive
```

### Codex

```bash
codex plugin marketplace add tokencanopy/agentdrive-plugin
```

For configuration guides across the other seven clients (Claude Desktop, Cursor, VS Code / Copilot, Windsurf, Gemini CLI, Zed, and any Agent Plugins 1.0 client), see [docs/add-to-your-agent.md](docs/add-to-your-agent.md).

## What's in here

| Directory / Path | Contents |
| --- | --- |
| `plugin/` | Agent Plugins 1.0 plugin (manifest, MCP config, skills, commands) |
| `connector/` | MCP Registry metadata + icons |
| `.claude-plugin/`, `.cursor-plugin/`, `.agents/` | Per-vendor marketplace manifests |
| `docs/` | Client configuration and integration guides |

## Not in here

The programmatic API client SDKs are maintained separately in [tokencanopy/agentdrive-sdk](https://github.com/tokencanopy/agentdrive-sdk):
- TypeScript / Node: `@tokencanopy/agentdrive-sdk` (npm)
- Python: `agentdrive-sdk` (PyPI)
- Go: `github.com/tokencanopy/agentdrive-sdk/sdk/go`

### Why this repo exists

The plugin previously lived inside `tokencanopy/agentdrive-sdk` alongside generated API clients. It was separated for two concrete reasons:
1. **No coupling.** Nothing in the plugin imports the SDKs; nothing in the SDKs mentions MCP. They are two distinct products.
2. **Incompatible release models.** The SDKs publish on tagged GitHub releases. Plugin marketplaces read a repository's default branch. Every routine SDK regeneration commit was instantly altering what installed plugin users received without version gates. Splitting allows this repository's default branch to move only when plugin manifests and capabilities actually change.

## Standard

This plugin conforms to [Agent Plugins 1.0](https://agent-plugins.org). The standard's two core files are `plugin/plugin.json` (plugin manifest) and `plugin/mcp.json` (MCP server definition). Claude Code is served by `plugin/.claude-plugin/` and `plugin/.mcp.json` because Anthropic is not among the standard's launch adopters.

## License

[MIT](LICENSE)
