# Event onCrmProductUpdate

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

{% note warning "Event development has been halted" %}

The event `onCrmProductUpdate` is still operational, but there is a more relevant alternative [CATALOG.PRODUCT.ON.UPDATE](../../../../catalog/product/events/catalog-product-on-update.md).

{% endnote %}

The event is triggered when a product is updated.

## Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **FIELDS** | The array contains the ID field with the value of the updated product's identifier ||
|#