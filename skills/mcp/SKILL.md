---
description: Connect Claude Code to xedi.ai MCP discovery, endpoint metadata and authenticated API tools. Use when a user asks Claude to discover XEDI capabilities, inspect available EDI APIs, create or validate connections, run XEDI MCP tools, or call XEDI workflow, mapping, parser, account or conversion endpoints.
---

# XEDI MCP

Use the bundled XEDI MCP server first. It is configured at:

```text
https://xedi.ai/mcp/server
```

The plugin asks the user for an XEDI API token as a sensitive plugin option and sends it as a bearer token.

## Discovery

When capabilities are unclear, discover them before guessing:

- MCP overview: `/mcp`
- MCP server card: `/.well-known/mcp/server-card.json`
- API catalog: `/.well-known/api-catalog`
- OpenAPI description: `/docs/openapi.yaml`

## Common MCP Tools

- `xedi.api_catalog` returns OpenAPI, MCP and agent discovery metadata.
- `xedi.api_request` calls authenticated `/api/v1` endpoints.
- `xedi.convert` converts between supported data formats.
- `xedi.parser` detects, parses, validates, converts and generates EDIFACT, X12, TRADACOMS and PEPPOL payloads.
- `xedi.mapping_apply` applies a saved mapping by UUID.
- `xedi.connection_types` returns connection field metadata, including required, optional and secret fields.
- `xedi.connection_create` creates encrypted workflow connections.
- `xedi.connections_list` lists saved connections and UUIDs for workflow `credential_id` fields.
- `xedi.connection_validate` runs live validation for saved connections.
- `xedi.workflow_node_types` returns valid workflow node types and graph-generation rules.
- `xedi.workflow_create` creates a saved workflow from server-side defaults.
- `xedi.workflow_generate` generates and saves a draft workflow from a workflow-building prompt.
- `xedi.workflow_validate` validates a workflow definition before saving or activating.
- `xedi.workflow_update` replaces workflow metadata and graph definition by UUID.
- `xedi.workflow_run` runs API-triggered workflows and can trigger webhook workflows when requested.
- `xedi.workflow_webhook` triggers webhook workflows whose source node reads from its configured connection.
- `xedi.account_summary` returns token, usage and subscription summaries.

## Operating Rules

- Prefer MCP tools over raw HTTP when the server exposes the needed capability.
- Use `xedi.api_catalog` or `xedi.workflow_node_types` before inventing payload shapes.
- Treat connection secrets as write-only. They are encrypted and are not returned by API or MCP responses.
- Keep workflow definitions in trigger-to-output node order because execution follows node order.
- Validate workflows before making them active.
- Do not expose TGMS connector internals, SFTP folder names or transport implementation details to end users.

