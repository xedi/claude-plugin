# XEDI Claude Code Plugin

Use this plugin to connect Claude Code to xedi.ai for EDI conversion, validation, partner mappings, saved workflows, and delivery automation.

## Setup

Install the plugin, then provide an XEDI API token when Claude Code prompts for plugin configuration. The token is stored as a sensitive Claude Code plugin option and passed to the bundled XEDI MCP server.

The plugin connects to:

```text
https://xedi.ai/mcp/server
```

## Included Skills

- `xedi:mcp` discovers and uses XEDI MCP/API tools.
- `xedi:edi-documents` converts, validates and routes business documents.
- `xedi:workflow-builder` creates and updates XEDI workflows from plain-English requests.

## Local Development

```bash
claude --plugin-dir /Users/ryan/Herd/claude-plugin-xedi
```

After edits, reload plugins inside Claude Code:

```text
/reload-plugins
```
