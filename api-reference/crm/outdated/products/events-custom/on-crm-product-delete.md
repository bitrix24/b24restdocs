# Event onCrmProductDelete

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% note warning "Event development has been halted" %}

The event `onCrmProductDelete` is still operational, but there is a more relevant alternative [CATALOG.PRODUCT.ON.DELETE](../../../../catalog/product/events/catalog-product-on-delete.md).

{% endnote %}

The event is triggered when a product is deleted.

## Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **FIELDS** | The array contains the ID field with the value of the deleted product's identifier ||
|#