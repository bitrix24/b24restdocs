# Event for Adding a New Recurring Invoice onCrmInvoiceRecurringAdd

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

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