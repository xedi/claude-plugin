---
description: Transform, validate and route EDI and business documents with xedi.ai. Use when a user asks to convert CSV, JSON, XML, IDoc or PDF data into EDIFACT, X12, TRADACOMS, PEPPOL or mapped delivery payloads; validate EDI documents; apply mappings; or prepare partner-aware outbound files.
---

# XEDI EDI Documents

Use XEDI APIs and MCP tools for conversion, validation, parsing, generation, mapping and delivery preparation. Prefer saved mappings and partner configuration over hand-authored envelope values. For the full MCP tool map, read `../../references/mcp-capabilities.md`.

## Core Workflows

- Import CSV, JSON, XML, SAP IDoc or extracted PDF data and transform it into target EDI payloads.
- Convert between JSON, CSV and XML using XEDI conversion endpoints.
- Convert SAP IDocs to and from JSON, CSV, EDIFACT, X12, TRADACOMS and mapped PEPPOL document payloads.
- Parse and validate EDIFACT, X12, TRADACOMS and PEPPOL payloads.
- Detect unknown EDI payloads before choosing parser or validation settings.
- Generate supported EDI payloads through `xedi.parser` when the user supplies mapped document JSON and required metadata.
- Apply partner-specific mapping validation where a mapping exists.
- Generate PEPPOL only from mapped document JSON and ensure Schematron validation passes before delivery.
- Use `/api/v1/{input_type}/{output_type}` or the equivalent MCP conversion tools for tracked conversion work.
- Use `xedi.docs_search` and `xedi.docs_get` to fetch source document templates before designing mappings or advising on customer exports.
- Source templates are available at `/docs/examples/source-documents.json` for order, order change, invoice, despatch advice, sales report and inventory report workflows in CSV, JSON and XML.
- Offline copies of those templates are bundled in `../../references/source-documents/`; use them when MCP or web docs are unavailable.

## Partner And Envelope Rules

- Mappings transform fields into a target document shape. They do not own trading partner envelopes, mailbox IDs, ANA qualifiers or TGMS generation counters.
- Trading partner document settings supply partner-specific envelope identifiers, document mappings and TGMS generation sequences.
- Select a trading partner and document type for every delivery workflow that needs partner envelope data, even when the final output is CSV, JSON or XML.
- Do not edit generated EDI interchange envelope values just to make a single delivery work. Fix the partner document configuration instead.
- Treat TGMS as a first-class destination. Never describe it to users as SFTP or as a folder layout.

## Document Types

Use the platform document type selected by the mapping or trading partner configuration. Common examples include:

- Order
- Order change
- ASN or despatch advice
- Invoice
- Stock or inventory report
- Sales report

When a user wants a reusable starter mapping, prefer generic document mappings for common TRADACOMS, EDIFACT and X12 document types, then let partner-specific settings override envelope and delivery concerns.
