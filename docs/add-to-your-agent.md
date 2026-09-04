# Add AgentDrive to your agent

AgentDrive is a remote MCP server (streamable HTTP, OAuth 2.1 PKCE, no API key):

```
https://drive.mcp.tokencanopy.com/mcp
```

---

## Claude Code

Install via marketplace:

```bash
claude plugin marketplace add tokencanopy/agentdrive-plugin
claude plugin install agentdrive@agentdrive
```

Direct MCP:

```bash
claude mcp add --transport http agentdrive https://drive.mcp.tokencanopy.com/mcp
```

## Claude Desktop/web

1. **Settings → Connectors → Add custom connector**
2. Name: `AgentDrive`
3. URL: `https://drive.mcp.tokencanopy.com/mcp`
4. Sign in when prompted.

## Codex

Install via marketplace:

```bash
codex plugin marketplace add tokencanopy/agentdrive-plugin
```

Direct remote MCP (`~/.codex/config.toml`):

```toml
[mcp_servers.agentdrive]
url = "https://drive.mcp.tokencanopy.com/mcp"
auth = "oauth"
enabled = true
```

Then:

```bash
codex mcp login agentdrive
```

## Cursor

Direct MCP config (`.cursor/mcp.json` or `~/.cursor/mcp.json`):

```json
{
  "mcpServers": {
    "agentdrive": {
      "type": "streamable-http",
      "url": "https://drive.mcp.tokencanopy.com/mcp"
    }
  }
}
```

Cursor also supports Agent Plugins 1.0.

## VS Code / Copilot

Direct MCP config (`.vscode/mcp.json` — note the key is `servers`, not `mcpServers`):

```json
{
  "servers": {
    "agentdrive": {
      "type": "http",
      "url": "https://drive.mcp.tokencanopy.com/mcp"
    }
  }
}
```

## Windsurf

Direct MCP config (`~/.codeium/windsurf/mcp_config.json`) — note the key is `serverUrl`:

```json
{
  "mcpServers": {
    "agentdrive": {
      "serverUrl": "https://drive.mcp.tokencanopy.com/mcp"
    }
  }
}
```

## Gemini CLI

Direct MCP config (`~/.gemini/settings.json` or `.gemini/settings.json`) — note the key is `httpUrl`:

```json
{
  "mcpServers": {
    "agentdrive": {
      "httpUrl": "https://drive.mcp.tokencanopy.com/mcp"
    }
  }
}
```

## Zed

Zed's native `context_servers` support for remote streamable-HTTP servers is
still inconsistent across releases. The reliable path is the `mcp-remote` stdio
bridge, which also handles the OAuth flow:

```json
{
  "context_servers": {
    "agentdrive": {
      "source": "custom",
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://drive.mcp.tokencanopy.com/mcp"]
    }
  }
}
```

If your Zed build accepts a direct `url` under `context_servers`, that works too
and skips the bridge.

## Any Agent Plugins 1.0 client

This repo ships an [Agent Plugins 1.0](https://agent-plugins.org) plugin at
`plugin/`, so any conformant client can load it directly from a checkout — no
per-vendor configuration. The standard's two files are `plugin/plugin.json`
(manifest) and `plugin/mcp.json` (MCP servers); skills live at
`plugin/skills/<name>/SKILL.md`. The standard defines no OAuth fields
deliberately: authorization discovery and credential storage are client-managed,
which is why none of the configs above carry a token.

## MCP Registry

Published as **`com.tokencanopy/agentdrive`**:

```
https://registry.modelcontextprotocol.io/v0.1/servers?search=com.tokencanopy/agentdrive
```

It was previously published as `run.agentdrive/agentdrive`, a namespace rooted
in a retired domain. That entry has been deleted and the listing migrated.

Note that an unrelated `sh.agentdrive/agentdrive` exists in the registry,
operated by someone else at a different endpoint. The `com.tokencanopy`
namespace is the one tied to the domain that actually serves this endpoint.
