# Event for Adding a New Recurring Invoice onCrmInvoiceRecurringAdd

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

This event is triggered when a new recurring invoice is created.

#|
|| **Name**
`type` | **Description** ||
|| **FIELDS** 
[`array`](../../../../data-types.md)| The array contains the following fields:
- **ID** — the value of the record identifier in the recurring invoice settings table
- **RECURRING_INVOICE_ID** — the value of the recurring invoice template identifier || 
|#