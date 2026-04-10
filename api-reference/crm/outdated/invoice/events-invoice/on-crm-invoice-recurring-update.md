# Event on Update Recurring Invoice onCrmInvoiceRecurringUpdate

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The event is triggered when a recurring invoice is updated.

## Parameters

{% include [Note on Required Parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **FIELDS** 
[`array`](../../../../data-types.md)| The array contains the following fields:
- **ID** — the value of the record identifier in the recurring invoice settings table
- **RECURRING_INVOICE_ID** — the value of the recurring invoice template identifier || 
|#