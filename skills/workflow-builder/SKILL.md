---
description: Build, validate and update xedi.ai saved workflows from plain-English requests. Use when a user asks Claude to create a workflow involving triggers, file pickup, transformation, mapping, validation, partner routing, TGMS, AS2, FTP, SFTP, email, Slack, Teams, webhooks or API-triggered delivery.
---

# XEDI Workflow Builder

Build workflows through XEDI MCP/API rather than hand-writing unsupported graph shapes. Prefer draft workflows until the user has supplied all production credentials and partner choices. For the full MCP tool map, read `../../references/mcp-capabilities.md`.

## Build Flow

1. Use `xedi.workflow_node_types` to discover valid nodes, required fields, optional fields and graph rules.
2. Use `xedi.docs_search` and `xedi.docs_get` when the workflow needs a base CSV, JSON or XML source document shape.
3. Normalise obvious spelling mistakes in the prompt before calling workflow tools, while preserving user intent.
4. If the user gives a plain-English workflow request, use `xedi.workflow_generate` to create a server draft.
5. Immediately run `xedi.workflow_validate` on the returned definition. Treat any validation error or warning about reachability, missing trigger/source/output, incompatible edges or missing required data as a blocker.
6. If the draft is invalid, fetch the current workflow with `xedi.workflow_get`, repair only with node types and fields from `xedi.workflow_node_types`, then call `xedi.workflow_validate` again.
7. Use `xedi.workflow_update` only after the repaired draft is valid, or when saving an explicitly incomplete draft with the remaining blockers clearly stated.
8. Create or update active workflows only when the user explicitly asks for active and validation passes.
9. Use `xedi.workflow_run_get` to inspect delivery, notification and EDI tracking results after execution.

## Prompt Normalisation

- Treat common typos as intent, not ambiguity: `tradacom`, `tradacomns` and `tradacoms` mean TRADACOMS; `edifcat` means EDIFACT; `tgms` means TGMS; `mapo` means map; `partbner` means partner.
- When sending a prompt to `xedi.workflow_generate`, rewrite corrected terms with exact platform words such as `TRADACOMS Order`, `SFTP`, `TGMS trading partner`, and `Slack notifications`.
- Preserve explicit folders exactly, including paths such as `/outgoing/orders/csv`.
- If the user asks for Slack or Teams notifications, add the notification node and let XEDI derive the message level from runtime outcome.

## Generation Recovery

- Do not present a broken generated graph as usable.
- Do not hand-build an unsupported graph from memory. Use `xedi.workflow_node_types` and the returned validation errors as the repair guide.
- Do not call XEDI mocked unless the tool response explicitly identifies the endpoint or route as mocked.
- If the requested standard is TRADACOMS, X12, EDIFACT or PEPPOL, preserve that target standard through mapping selection, conversion target and validation format.
- If no mapping matches the requested source, target standard and document type, ask the user to choose or create one rather than selecting a different EDI standard.

## Defaults

- Let server-side defaults fill unambiguous single choices for connection, notification, trading partner, TGMS document type and partner document mapping.
- Use connection UUIDs as workflow node `credential_id` values.
- Keep one trigger node, and make sure the first node matches the workflow `trigger_type`.
- Keep nodes in execution order.
- Space generated node positions horizontally so nodes do not overlap.

## File Pickup Patterns

- SFTP arrival trigger: use `trigger_type=sftp_file_arrived` with first node `trigger.sftp_file_arrived`; set `credential_id`, absolute `folder`, optional `file_pattern`, and post-processing fields.
- FTP arrival trigger: use `trigger_type=ftp_file_arrived` with first node `trigger.ftp_file_arrived`.
- Scheduled or manual folder polling: use `trigger.cron` or `trigger.manual` followed by `source.connection` with `credential_id`, `source_folder`, `file_selection`, and post-processing fields.
- Webhook-triggered file reads: use `trigger.webhook` followed by `source.connection`.
- API-triggered workflows should only use `trigger.api` and should not add `source.connection` unless the API request is intentionally ignored in favour of a configured source.

## Mapping And Format Selection

- Source document templates are available through `xedi.docs_search`, `xedi.docs_get` and `/docs/examples/source-documents.json`.
- Offline copies are bundled in `../../references/source-documents/`.
- Use the source templates as stable field-name context for order, order change, invoice, despatch advice, sales report and inventory report exports.
- `convert.format.data.target` uses broad format tokens such as `edifact`, `x12`, `tradacoms`, `peppol`, `json`, `csv` or `xml`.
- `mapping_id` supplies the document-specific transform. Its `destination_format` and `document_type` must match the requested target standard and business document.
- If the user says TRADACOMS, do not choose an EDIFACT mapping. If the user says EDIFACT, do not choose a TRADACOMS mapping.
- For "CSV orders to TRADACOMS order", use `source=csv`, `target=tradacoms`, a mapping whose destination is TRADACOMS, and document type `order`.
- For TGMS, prefer the mapping configured on the selected trading partner document sequence when one exists.

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
- XEDI automatically derives notification severity: `success` for completed workflows, `warning` for non-fatal validation or warning conditions, and `error` for failed workflow runs or failed upstream nodes.
- Let users choose which fields appear in messages, defaulting all available fields to included.

## User Experience Rules

- Do not expose TGMS as SFTP.
- Do not mention internal base folders or subfolder layouts for TGMS.
- Prefer follow-up questions only when a missing value blocks safe workflow creation. Otherwise create a draft workflow the user can edit.
- Use `xedi.api_request` for workflow run list filters until a dedicated list wrapper exists.
