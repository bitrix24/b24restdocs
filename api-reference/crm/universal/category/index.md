# Sales Funnels: Overview of Methods

The methods `crm.category.*` manage the sales funnels of CRM entities that support categories: they create new funnels, modify settings, retrieve data by `id` or a list of funnels, delete a funnel, and return the composition of fields.

Funnels are used to separate work by departments or types of sales. They are most commonly configured for deals and Smart Process Automation (SPA). For example, to add a deal to a specific funnel, you retrieve the funnel's `id` using the [crm.category.list](./crm-category-list.md) method and pass it as `categoryId` in the [crm.item.add](../crm-item-add.md) method.

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Quick Navigation: [All Methods](#all-methods)
>
> User Documentation: [Sales pipelines](https://helpdesk.bitrix24.com/open/20739996/)

## Getting Started

1. Create a funnel using the [crm.category.add](./crm-category-add.md) method.

2. Link the funnel to the desired CRM entity in the [crm.item.add](../crm-item-add.md) or [crm.item.update](../crm-item-update.md) methods by passing the funnel's `id` as `categoryId`.

3. To check the settings of a specific funnel, retrieve its data by `id` using the [crm.category.get](./crm-category-get.md) method.

4. If you need to change the funnel parameters, use the [crm.category.update](./crm-category-update.md) method.

5. To get a list of funnels, use the [crm.category.list](./crm-category-list.md) method.

6. The available fields are returned by the [crm.category.fields](./crm-category-fields.md) method.

7. To delete a funnel, use the [crm.category.delete](./crm-category-delete.md) method.

{% note tip "Typical use-cases and scenarios" %}

- [How to Create a New Funnel with Stages in a Smart Process](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-category-to-spa.md)
- [How to Filter Items by Stage Name](../../../../tutorials/crm/how-to-get-lists/how-to-get-elements-by-stage-filter.md)
- [How to Create a Vendor](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-contractor.md)
- [How to Get a List of Vendors](../../../../tutorials/crm/how-to-get-lists/how-to-get-contractors.md)

{% endnote %}

## Important Considerations

**Access Permissions.** The [crm.category.list](./crm-category-list.md) method is available to any user but only returns those funnels for which the user has read access. Methods for creating, modifying, and deleting require administrative access to CRM.

**Default Funnel.** The `isDefault` field behaves differently depending on the entity. In deals, it cannot be changed. In Smart Processes, a new default funnel can be assigned, causing the old one to lose that status. The `isDefault` flag cannot be removed from the current default funnel.

**Deleting a Funnel.** The [crm.category.delete](./crm-category-delete.md) method will return an error if the funnel is the default funnel or contains elements.

## Relationship with Other Entities

**Deals.** The [crm.category.list](./crm-category-list.md) method works with the funnels of [deals](../../deals/index.md). In deals, the funnel is linked to the entity through the `categoryId` field.

**Smart Processes.** Funnels in a Smart Process work if the object type has the `isCategoriesEnabled` option enabled. You can check the setting and retrieve the `entityTypeId` of the Smart Process using the [crm.type.list](../user-defined-object-types/crm-type-list.md) method.

**Stages.** Each funnel defines its own reference of stages with a unique `ENTITY_ID`. To work with stages, use the methods in the [CRM Reference](../../status/index.md). The identifiers of the references are returned by the [crm.status.entity.types](../../status/crm-status-entity-types.md) method.

**Deal Cards.** The settings of deal cards depend on the funnel. To configure a card for a specific funnel, pass the funnel's `id` as `dealCategoryId` in the [crm.deal.details.configuration.get](../../deals/custom-form/crm-deal-details-configuration-get.md) and [crm.deal.details.configuration.set](../../deals/custom-form/crm-deal-details-configuration-set.md) methods.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#| 
|| **Method** | **Description** ||
|| [crm.category.add](./crm-category-add.md) | Creates a new funnel ||
|| [crm.category.update](./crm-category-update.md) | Updates a funnel ||
|| [crm.category.get](./crm-category-get.md) | Returns a funnel by `id` ||
|| [crm.category.list](./crm-category-list.md) | Returns a list of funnels ||
|| [crm.category.delete](./crm-category-delete.md) | Deletes a funnel ||
|| [crm.category.fields](./crm-category-fields.md) | Returns the description of funnel fields ||
|#