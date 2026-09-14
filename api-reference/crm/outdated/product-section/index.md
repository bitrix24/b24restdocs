# Product Sections: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% note warning "" %}

**DEPRECATED**

The development of methods crm.productsection.* has been halted.  
Please use the section [Catalog Sections (`catalog.section.*`)](../../../catalog/section/index.md).

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [crm.productsection.add](./crm-product-section-add.md) | Creates a new product section. ||
|| [crm.productsection.delete](./crm-product-section-delete.md) | Deletes a product section. ||
|| [crm.productsection.fields](./crm-product-section-fields.md) | Returns the description of the product section fields. ||
|| [crm.productsection.get](./crm-product-section-get.md) | Returns a product section by its identifier. ||
|| [crm.productsection.list](./crm-product-section-list.md) | Returns a list of product sections based on a filter. ||
|| [crm.productsection.update](./crm-product-section-update.md) | Updates an existing product section. ||
|#