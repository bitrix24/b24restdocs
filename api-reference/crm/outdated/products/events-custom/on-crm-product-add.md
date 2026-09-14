# Event onCrmProductAdd

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

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