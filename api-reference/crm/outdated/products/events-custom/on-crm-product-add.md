# Event onCrmProductAdd

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

{% note warning "Event development has been halted" %}

The event `onCrmProductAdd` is still operational, but it has a more relevant counterpart [CATALOG.PRODUCT.ON.ADD](../../../../catalog/product/events/catalog-product-on-add.md).

{% endnote %}

The event is triggered when a product is created.

## Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **FIELDS** | The array contains the field ID with the value of the created product's identifier ||
|#