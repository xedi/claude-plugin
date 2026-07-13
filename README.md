<p align="center">
  <img src="assets/logo.svg" alt="XEDI" width="180">
</p>

<h1 align="center">XEDI Claude Code Plugin</h1>

<p align="center">
  Give Claude Code native guidance for the full xedi.ai MCP surface: EDI conversion, validation, mappings, workflows, connections, labels, usage, billing and partner-aware delivery.
</p>

<p align="center">
  <a href="https://xedi.ai/docs">Docs</a>
  ·
  <a href="https://xedi.ai/mcp">MCP</a>
  ·
  <a href="https://xedi.ai">xedi.ai</a>
</p>

---

## What It Does

This plugin helps Claude Code work with xedi.ai as an EDI automation platform. It adds focused skills for:

- Discovering and using the XEDI MCP server.
- Converting between business data formats and EDI standards.
- Validating EDIFACT, X12, TRADACOMS and PEPPOL payloads.
- Applying reusable mappings without mixing them up with partner envelopes.
- Managing encrypted workflow connections and browsing source folders.
- Listing and fetching SSCC pallet label templates.
- Building draft workflows from plain-English prompts.
- Inspecting workflow runs, notification reports and delivery tracking.
- Reading usage, token balance and subscription information.
- Routing documents through partner-aware delivery destinations such as TGMS, AS2, FTP, SFTP, email, Slack and Teams.
- Calling any authenticated `/api/v1` endpoint through `xedi.api_request` when the platform exposes a capability before it has a named MCP wrapper.

## Included Skills

| Skill | Use It For |
| --- | --- |
| `xedi:mcp` | XEDI MCP discovery, authenticated API tools, connections, mappings, workflows and account usage. |
| `xedi:platform-operations` | Full platform operations: usage, tokens, subscriptions, connections, labels, mappings, workflow runs and generic `/api/v1` calls. |
| `xedi:edi-documents` | Document conversion, validation, mapping, EDI envelope guidance and partner-aware delivery preparation. |
| `xedi:workflow-builder` | Creating, validating and updating XEDI workflows from natural-language requests. |

## MCP Connection

The plugin includes an MCP server definition for:

```text
https://xedi.ai/mcp/server
```

When installed, Claude Code asks for an XEDI API token as sensitive plugin configuration. The token is sent as:

```http
Authorization: Bearer <your-xedi-api-token>
```

## Local Install

Clone this repository, then run Claude Code with the plugin directory:

```bash
git clone git@github.com:xedi/claude-plugin.git
cd claude-plugin
claude --plugin-dir .
```

During development, reload the plugin inside Claude Code after edits:

```text
/reload-plugins
```

## Example Prompts

```text
Use XEDI to create a draft workflow that picks up JSON orders from SFTP,
maps them to EDIFACT ORDERS and delivers them to my TGMS partner.
```

```text
Validate this EDIFACT invoice and tell me whether it matches the selected
trading partner rules.
```

```text
List my XEDI connections and build a workflow using the only available AS2
destination if it is unambiguous.
```

```text
Show my XEDI usage summary, token balance and active managed-service
subscriptions.
```

```text
Browse the selected SFTP connection folder and help me choose the source path
for a workflow trigger.
```

```text
Create a reusable invoice mapping for JSON to EDIFACT, but keep envelope
values on the trading partner document settings.
```

## Design Principles

- TGMS is treated as a first-class XEDI destination. Users should not need to know about its underlying transport.
- Mappings transform fields. Trading partners own envelopes, identifiers, ANA qualifiers and generation-number sequences.
- Workflow generation starts as draft unless the user has supplied all production choices and validation passes.
- Claude should discover node types, connection fields and API shapes through XEDI MCP instead of guessing.
- Dedicated MCP tools are preferred, with `xedi.api_request` used as the escape hatch for the full authenticated `/api/v1` surface.

## Repository Layout

```text
.
├── .claude-plugin/plugin.json
├── .mcp.json
├── assets/
│   ├── logo.png
│   └── logo.svg
├── references/
│   └── mcp-capabilities.md
└── skills/
    ├── edi-documents/SKILL.md
    ├── mcp/SKILL.md
    ├── platform-operations/SKILL.md
    └── workflow-builder/SKILL.md
```

## Validate

```bash
claude plugin validate . --strict
```

## Community Marketplace

This repository is prepared for Claude Code plugin distribution. For community marketplace submission, validate the plugin locally, then submit the GitHub repository through Anthropic's Claude Code plugin process.

## License

This plugin is licensed under the [Apache License 2.0](LICENSE).

XEDI, xedi.ai, the XEDI name, and the XEDI logos are trademarks or registered trademarks of XEDI Ltd. See [NOTICE](NOTICE) for trademark and attribution details.
