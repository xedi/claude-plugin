---
description: Connect Claude Code to the full xedi.ai MCP and authenticated API surface. Use when a user asks Claude to discover XEDI capabilities; inspect EDI, workflow, mapping, connection, label, usage, token, subscription or account APIs; run XEDI MCP tools; or call any authenticated /api/v1 platform endpoint.
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

## Capability Reference

For the full current tool map, read `../../references/mcp-capabilities.md`.

## MCP Tool Areas

- `xedi.api_catalog` returns OpenAPI, MCP and agent discovery metadata.
- `xedi.api_request` calls any authenticated `/api/v1` endpoint when a dedicated wrapper is not available.
- Discovery tools cover health, supported formats and conversion routes.
- Document tools cover conversion, EDI validation, parser detection, parsing, conversion and generation.
- Label tools list and fetch SSCC pallet label templates.
- Mapping tools list, create, fetch, update, delete, apply and validate saved mappings.
- Connection tools list types, create/update/delete encrypted connections, fetch connections without secrets, validate live credentials and browse folders.
- Workflow tools list, create, generate, fetch, update, archive, validate, run, trigger webhooks and fetch run detail.
- Account tools report usage, token balance, token top-ups, subscription products, subscription creation and account summary.

## Operating Rules

- Prefer MCP tools over raw HTTP when the server exposes the needed capability.
- Use `xedi.api_catalog` before inventing payload shapes.
- Use `xedi.workflow_node_types` before creating or updating workflow graphs.
- Treat connection secrets as write-only. They are encrypted and are not returned by API or MCP responses.
- Keep workflow definitions in trigger-to-output node order because execution follows node order.
- Validate workflows before making them active.
- Ask for confirmation before purchase, subscription, archive or delete actions.
- Do not expose TGMS connector internals, SFTP folder names or transport implementation details to end users.
