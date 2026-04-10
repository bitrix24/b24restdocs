# Event for Adding Custom Field onCrmInvoiceUserFieldAdd

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The event is triggered when a custom field is added.

## Parameters

{% include [Note on Required Parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id** 
[`integer`](../../../../data-types.md)| Identifier of the custom field ||
|| **entityId** 
[`string`](../../../../data-types.md)| Symbolic identifier of the entity for which the field was created ||
|| **fieldName** 
[`string`](../../../../data-types.md)| Name of the created custom field ||
|#