# Deal Directions

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

{% note warning "" %}

**DEPRECATED**

The development of methods crm.deal.category.* has been halted. 
Please use the section [Sales Funnels (`crm.category.*`)](../../universal/category/index.md).

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: any user

#|
|| **Method** | **Description** ||
|| [crm.deal.category.add](./crm-deal-category-add.md) | Creates a new deal direction ||
|| [crm.deal.category.delete](./crm-deal-category-delete.md) | Deletes a deal direction ||
|| [crm.deal.category.get](./crm-deal-category-get.md) | Retrieves a deal direction by its identifier ||
|| [crm.deal.category.fields](./crm-deal-category-fields.md) | Gets the fields of the invoice status ||
|| [crm.deal.category.list](./crm-deal-category-list.md) | Retrieves the description of deal direction fields ||
|| [crm.deal.category.update](./crm-deal-category-update.md) | Updates an existing deal direction ||
|| [crm.deal.category.default.get](./crm-deal-category-default-get.md) | Gets the settings for the default deal direction ||
|| [crm.deal.category.default.set](./crm-deal-category-default-set.md) | Sets the settings for the default deal direction ||
|| [crm.deal.category.status](./crm-deal-category-status.md) | Returns the identifier of the directory where deal stages are stored ||
|| [crm.deal.category.stage.list](./crm-deal-category-stage-list.md) | Returns a list of deal stages for the direction ||
|#