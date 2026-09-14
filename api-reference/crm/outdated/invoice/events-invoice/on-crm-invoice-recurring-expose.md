# Event for Issuing a New Invoice from Regular onCrmInvoiceRecurringExpose

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The event is triggered when a new invoice is issued from a recurring invoice.

## Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **FIELDS** 
[`array`](../../../../data-types.md)| The array contains the following fields:
- **ID** — the identifier value of the record in the recurring invoice settings table
- **RECURRING_INVOICE_ID** — the identifier value of the recurring invoice template
- **INVOICE_ID** — the ID value of the new invoice created based on the recurring invoice || 
|#