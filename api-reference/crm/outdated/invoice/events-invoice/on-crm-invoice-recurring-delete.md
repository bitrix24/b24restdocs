# Event for Deleting Recurring Invoice onCrmInvoiceRecurringDelete

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

This event is triggered when a recurring invoice is deleted.

## Parameters

{% include [Note on Required Parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **FIELDS** 
[`array`](../../../../data-types.md)| The array contains the field ID with the value of the record identifier in the recurring invoice settings table ||
|#