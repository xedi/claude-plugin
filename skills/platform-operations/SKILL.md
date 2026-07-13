---
description: Operate the broader xedi.ai platform through MCP, not just EDI conversion. Use when a user asks Claude to manage or inspect XEDI account usage, tokens, subscriptions, connections, connection folders, label templates, mappings, workflow runs, API catalog metadata, or any authenticated /api/v1 platform endpoint.
---

# XEDI Platform Operations

Use the XEDI MCP server as the platform control surface. For a full capability map, read `../../references/mcp-capabilities.md`.

## General Flow

1. Use `xedi.api_catalog` when endpoint or payload shape is unclear.
2. Prefer a dedicated MCP tool when one exists.
3. Use `xedi.api_request` for authenticated `/api/v1` endpoints without a dedicated wrapper.
4. Confirm before destructive, billing or purchase actions.
5. Treat secrets as write-only and never expect them to be returned.

## Platform Areas

- Discovery and health: API catalog, formats, conversion routes and health checks.
- Documents: conversion, parser detection, parsing, validation, generation and dynamic format conversion.
- Mappings: list, create, fetch, update, delete, apply and validate mappings.
- Connections: list types, create/update encrypted connections, validate live credentials and browse folders.
- Workflows: list, create, generate, validate, update, archive, run, trigger webhooks and inspect run detail.
- Labels: list and fetch SSCC pallet label templates.
- Account: usage summaries, token balances, token top-ups, subscriptions and account summaries.

## Guardrails

- Ask for confirmation before `xedi.tokens_top_up`, `xedi.subscription_create`, `xedi.mapping_delete`, `xedi.connection_delete` or `xedi.workflow_delete`.
- Use list/get tools before updating existing resources so UUIDs and current values are known.
- Do not display connection secrets. If a secret must be updated, ask the user to provide the new value and send it only to the relevant create/update call.
- Explain when a capability is available only through `xedi.api_request` rather than a named MCP wrapper.

