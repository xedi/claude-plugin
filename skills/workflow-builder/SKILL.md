---
description: Build, validate and update xedi.ai saved workflows from plain-English requests. Use when a user asks Claude to create a workflow involving triggers, file pickup, transformation, mapping, validation, partner routing, TGMS, AS2, FTP, SFTP, email, Slack, Teams, webhooks or API-triggered delivery.
---

# XEDI Workflow Builder

Build workflows through XEDI MCP/API rather than hand-writing unsupported graph shapes. Prefer draft workflows until the user has supplied all production credentials and partner choices. For the full MCP tool map, read `../../references/mcp-capabilities.md`.

## Build Flow

1. Use `xedi.workflow_node_types` to discover valid nodes, required fields, optional fields and graph rules.
2. If the user gives a plain-English workflow request, use `xedi.workflow_generate`.
3. Create or update the workflow as a draft unless the user explicitly asks for active and validation passes.
4. Run `xedi.workflow_validate` before activation.
5. Use `xedi.workflow_update` to replace metadata or graph definitions by UUID.
6. Use `xedi.workflow_run_get` to inspect delivery, notification and EDI tracking results after execution.

## Defaults

- Let server-side defaults fill unambiguous single choices for connection, notification, trading partner, TGMS document type and partner document mapping.
- Use connection UUIDs as workflow node `credential_id` values.
- Keep one trigger node, and make sure the first node matches the workflow `trigger_type`.
- Keep nodes in execution order.
- Space generated node positions horizontally so nodes do not overlap.

## Partner-Aware Delivery

- For TGMS, AS2, FTP, SFTP, email or notification deliveries that carry business documents, select the trading partner and document type when the workflow needs envelope data.
- Use partner document mappings rather than mapping-owned envelope settings.
- TGMS delivery requires trading partner and document type configuration. Generation number allocation happens at execution time and is recorded in delivery logs.
- Retry delivery reuses the same generation number and version.
- Resend as new version reuses the generation number and atomically allocates the next version.
- Send as new document allocates a new generation number and starts at version 1.

## Notifications

- Slack and Teams can be webhook-only or full-integration notifications.
- Webhook-only notifications can send text and secure links; full integrations can attach files where the platform supports file upload.
- Use notification types such as success, warning, error and information to choose message tone, colour and icon.
- Let users choose which fields appear in messages, defaulting all available fields to included.

## User Experience Rules

- Do not expose TGMS as SFTP.
- Do not mention internal base folders or subfolder layouts for TGMS.
- Prefer follow-up questions only when a missing value blocks safe workflow creation. Otherwise create a draft workflow the user can edit.
- Use `xedi.api_request` for workflow run list filters until a dedicated list wrapper exists.
