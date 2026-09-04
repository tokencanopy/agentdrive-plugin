# Contributor & Agent Guide: `agentdrive-plugin`

This guide explains how this repository is structured and the rules for modifying it.

## What this repository is

`tokencanopy/agentdrive-plugin` is the standalone repository hosting the AgentDrive agent plugin, vendor marketplace manifests, and MCP connector metadata. AgentDrive is developed by Mnexa, Inc.

The programmatic API client SDKs (TypeScript, Python, Go) live in a separate repository: `tokencanopy/agentdrive-sdk`.

## No build or test suite

There is no build step and no test suite beyond manifest and liveness validation. CI runs `.github/workflows/validate.yml`, which verifies:
1. Every JSON file parses as valid JSON.
2. `plugin/plugin.json` strictly adheres to Agent Plugins 1.0 allowed top-level keys.
3. The remote MCP endpoint answers with HTTP 401 (verifying liveness and OAuth protection).

## Repository Layout

- `plugin/` — The primary Agent Plugins 1.0 directory. Contains `plugin.json` (plugin manifest), `mcp.json` (MCP server definition), `commands/`, `skills/`, `assets/`, and vendor-specific plugin manifests (`.claude-plugin/`, `.codex-plugin/`, `.cursor-plugin/`, `.mcp.json`).
- `connector/` — MCP Registry submission metadata (`server.json`), icons (`connector-icon*`), and `llms.txt`.
- `.claude-plugin/`, `.cursor-plugin/`, `.agents/` — Per-vendor marketplace manifests pointing to `./plugin`.
- `docs/` — User and agent integration documentation (`docs/add-to-your-agent.md`).
- `.github/workflows/` — Automated validation workflow.

## Hard Rules

### 1. Consistent MCP Endpoint
The MCP endpoint URL is:
```
https://drive.mcp.tokencanopy.com/mcp
```
This URL appears in multiple files:
- `connector/server.json`
- `connector/llms.txt`
- `plugin/mcp.json`
- `plugin/.mcp.json`
- `docs/add-to-your-agent.md`
- `README.md`

It **must stay identical in all of them**. Never update one without updating the rest, and never change it without explicit authorization.

### 2. MCP Registry Submission (`connector/server.json`)
`connector/server.json` is the MCP Registry submission metadata. Publishing or updating an entry on the MCP Registry is a manual, human action requiring DNS-verified domain ownership. Modifying `connector/server.json` in git does **not** update the live registry listing. Do not change the `name` field in `connector/server.json`.

### 3. Agent Plugins 1.0 Schema Conformance
`plugin/plugin.json` conforms strictly to the [Agent Plugins 1.0](https://agent-plugins.org) specification. The specification explicitly forbids unrecognized top-level keys. Only these keys are permitted:
`$schema`, `name`, `version`, `description`, `author`, `homepage`, `repository`, `license`, `keywords`, `extensions`.
Vendor-specific fields belong in their respective vendor subdirectories (e.g., `plugin/.claude-plugin/`, `plugin/.codex-plugin/`, `plugin/.cursor-plugin/`).

### 4. Ground Truth URLs and Contacts
- MCP Server: `https://drive.mcp.tokencanopy.com/mcp`
- REST API Base (v0): `https://drive.tokencanopy.com`
- Console / Sign-in: `https://app.tokencanopy.com`
- Website: `https://tokencanopy.com`
- Contact: `hello@mnexa.ai`
