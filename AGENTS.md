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

#### Versioning

The registry enforces two rules that make version choice irreversible, so get it
right before publishing:

1. **Once published, a version string and its metadata cannot be changed.**
2. **Versions are sorted by semver to decide which is `latest`.** Publishing a
   version that sorts *below* the current one does not make it latest — it lands
   as an orphaned entry while the higher version stays canonical. There is no
   way to correct a version number after the fact except deleting the entry and
   republishing.

Rules for picking the next one:

- **Track the remote API version.** AgentDrive's API is `v0`, so the server
  version stays in `0.x`. When the API moves to `v1`, this moves to `1.x`.
- **Metadata-only republishes** (description, icons, `websiteUrl`) still need a
  new version. Increment the patch — `0.1.1`, `0.1.2`. The registry documents
  prerelease strings (`0.2.0-1`) for this, but they sort *before* their release
  version, so they only work if you publish the prereleases first and the plain
  version last.
- The published entry is **`com.tokencanopy/agentdrive` `0.1.0`**, serving
  `https://drive.mcp.tokencanopy.com/mcp`. Publishing requires a DNS-verified
  key on `tokencanopy.com`.
- The former `run.agentdrive/agentdrive` entry is **deleted**. Note why it had
  to be deleted rather than left in place: the registry allows a remote URL to
  be claimed by exactly **one** server name, and neither deprecating nor
  leaving the old entry releases that claim. A namespace migration is therefore
  delete-then-publish, not publish-then-deprecate.

Note that `0.1.0` was a minor bump for what was really a patch-level URL
correction. That was a mistake, kept because it cannot be edited and republishing
lower would not take. Do not read it as signalling a feature.

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
