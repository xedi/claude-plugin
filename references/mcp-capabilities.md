# XEDI MCP Capability Reference

Use this reference when a user asks what XEDI can do, when choosing the right MCP tool, or when a task goes beyond conversion and workflow generation.

## Discovery And Fallback

- `xedi.api_catalog`: Return OpenAPI, MCP and agent discovery metadata.
- `xedi.api_request`: Call any authenticated `/api/v1` endpoint. Use this when the API supports a capability that does not yet have a dedicated MCP wrapper.
- `xedi.health`: Check XEDI API availability.
- `xedi.formats_list`: List supported source and target formats.
- `xedi.conversions_list`: List supported conversion routes, actions, paths and native or mocked status.

Always prefer a dedicated MCP tool when one exists. Use `xedi.api_catalog` before guessing endpoint shapes. Use `xedi.api_request` for newer platform API endpoints, list endpoints without dedicated wrappers, or compatibility with the full `/api/v1` surface.

## Conversion, Validation And Parsing

- `xedi.convert_generic`: Convert through `/api/v1/convert` with explicit source and target formats.
- `xedi.convert`: Convert between supported formats such as CSV, JSON, XML, EDIFACT, X12, TRADACOMS, PEPPOL, IDoc and PDF.
- `xedi.validate`: Validate supported EDI data through the validation API.
- `xedi.parser`: Detect, parse, validate, convert or generate supported EDI payloads.

Supported parser/validator coverage currently includes EDIFACT `ORDERS`, `ORDCHG`, `INVOIC`, `DESADV`, `SLSRPT` and `INVRPT`; X12 `850`, `860`, `810`, `856`, `852` and `846`; supported TRADACOMS order, customer order, invoice, credit and ASN families; and supported PEPPOL Billing Invoice, Billing Credit Note, Order, Order Change and Despatch Advice profiles.

## Label Templates

- `xedi.label_templates_list`: List saved SSCC pallet label templates.
- `xedi.label_template_get`: Fetch a saved SSCC pallet label template by UUID.

Use label templates for ASN and logistics label generation workflows. Fetch the template before assuming its fields or layout.

## Mappings

- `xedi.mappings_list`: List saved mappings.
- `xedi.mapping_get`: Fetch a saved mapping by UUID.
- `xedi.mapping_create`: Create a saved field mapping.
- `xedi.mapping_update`: Replace a saved mapping by UUID.
- `xedi.mapping_delete`: Delete a saved mapping by UUID.
- `xedi.mapping_apply`: Apply a saved mapping by UUID.
- `xedi.mapping_validate`: Validate an EDI payload and check whether a saved mapping can read its configured source fields.

Mappings are reusable transformation definitions. They do not own trading partner envelopes, mailbox IDs, ANA qualifiers, delivery folders or TGMS generation counters.

## Connections

- `xedi.connection_types`: List connection types, required fields, optional fields, secret fields and secret-handling rules.
- `xedi.connections_list`: List saved workflow connections without secrets.
- `xedi.connection_create`: Create an encrypted workflow connection.
- `xedi.connection_get`: Fetch a saved workflow connection by UUID without secrets.
- `xedi.connection_update`: Update an encrypted workflow connection by UUID. Existing secrets are retained when omitted.
- `xedi.connection_delete`: Delete a workflow connection by UUID.
- `xedi.connection_validate`: Run live validation for a workflow connection.
- `xedi.connection_browse`: Browse folders for a workflow connection.

Always inspect `xedi.connection_types` before creating or updating a connection. Treat secrets as write-only.

## Workflows And Runs

- `xedi.workflows_list`: List saved workflows.
- `xedi.workflow_create`: Create a saved workflow from server-side defaults.
- `xedi.workflow_generate`: Generate and save a draft workflow from a plain-English workflow-building prompt.
- `xedi.workflow_get`: Fetch a saved workflow by UUID.
- `xedi.workflow_update`: Replace workflow metadata and graph definition by UUID.
- `xedi.workflow_delete`: Archive a workflow by UUID.
- `xedi.workflow_node_types`: List node types, required fields, optional fields, trigger types and graph-generation rules.
- `xedi.workflow_validate`: Validate a workflow graph before saving or activating it.
- `xedi.workflow_run`: Run an API-triggered or webhook-triggered workflow by UUID.
- `xedi.workflow_run_get`: Fetch a workflow run by UUID, including notification reports and EDI delivery tracking.
- `xedi.workflow_webhook`: Trigger a webhook workflow by UUID.

Use `xedi.api_request` for workflow run list queries such as `GET /api/v1/workflow-runs?status=failed&retryable=1` when a dedicated list tool is not available.

Workflow drafts are not reliable until validated. After `xedi.workflow_generate`, call `xedi.workflow_validate`. Repair invalid graphs using `xedi.workflow_node_types`, not guessed node types. The first node must be the trigger node matching `trigger_type`, and every node must be reachable from that trigger.

## Usage, Tokens And Subscriptions

- `xedi.usage_summary`: Return API usage totals, endpoint usage and token estimates.
- `xedi.tokens_balance`: Return remaining prepaid tokens, used tokens, purchased tokens and auto top-up settings.
- `xedi.tokens_top_up`: Purchase prepaid token packs using the default payment method.
- `xedi.subscriptions_list`: List managed service subscription products, prices, included tokens and active status.
- `xedi.subscription_create`: Subscribe to a managed XEDI service using the default payment method.
- `xedi.account_summary`: Return authenticated account usage, token balance and available subscription products.

Treat billing and purchase actions as high-impact. Confirm intent before calling tools that purchase tokens or create subscriptions.

## Useful `/api/v1` Endpoints Through `xedi.api_request`

- `GET /health`
- `GET /formats`
- `GET /conversions`
- `GET /mappings`
- `POST /mappings`
- `GET /mapping/{uuid}`
- `PUT /mapping/{uuid}`
- `DELETE /mapping/{uuid}`
- `POST /mapping/{uuid}/apply`
- `GET /label-templates`
- `GET /label-template/{uuid}`
- `GET /connection-types`
- `GET /connections`
- `POST /connections`
- `GET /connection/{uuid}`
- `PUT /connection/{uuid}`
- `DELETE /connection/{uuid}`
- `POST /connection/{uuid}/validate`
- `GET /connection/{uuid}/browse`
- `GET /workflows`
- `POST /workflows`
- `POST /workflows/generate`
- `GET /workflow-node-types`
- `POST /workflow/validate`
- `GET /workflow/{uuid}`
- `PUT /workflow/{uuid}`
- `DELETE /workflow/{uuid}`
- `POST /workflow/{uuid}`
- `POST /workflow/{uuid}/webhook`
- `GET /workflow-runs`
- `GET /workflow-runs/{uuid}`
- `POST /convert`
- `GET /parser/health`
- `POST /parser/detect`
- `POST /parser/parse`
- `POST /parser/validate`
- `POST /parser/validate/{mapping_uuid}`
- `POST /parser/convert`
- `POST /parser/generate`
- `POST /{input_type}/{output_type}`
- `GET /usage`
- `GET /tokens`
- `POST /tokens/top-up`
- `GET /subscriptions`
- `POST /subscriptions`
