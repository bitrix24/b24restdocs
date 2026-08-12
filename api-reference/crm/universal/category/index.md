# Sales Funnels: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The methods `crm.category.*` manage funnels. A funnel is a separate branch of work with a CRM object that has its own set of stages and its own card settings.

Funnels are used to separate work by departments or types of sales. They are most commonly configured for deals and Smart Process Automation (SPA). For example, to add a deal to a specific funnel, you retrieve the funnel's `id` using the [crm.category.list](./crm-category-list.md) method and pass it as `categoryId` in the [crm.item.add](../crm-item-add.md) method.

> Quick Navigation: [All Methods](#all-methods)
>
> User Documentation: [Sales pipelines](https://helpdesk.bitrix24.com/open/20739996/)

## Which Objects Support Funnels

The object for which a funnel is needed is specified by the `entityTypeId` parameter. Not all CRM objects support funnels.

#|
|| **Object** | **`entityTypeId`** ||
|| Deal | `2` ||
|| Contact | `3` ||
|| Company | `4` ||
|| Invoice | `31` ||
|| Smart Process | from `128` ||
|#

The smart process identifier is returned by the [crm.type.list](../user-defined-object-types/crm-type-list.md) method, and the complete table of types is available in the [CRM object types reference](../../data-types.md#object_type). For an object without funnel support, such as a lead, the `crm.category.*` methods will return the `ENTITY_TYPE_NOT_SUPPORTED` error.

## Getting Started

1. Determine the `entityTypeId` of the object for which the funnel is needed.
2. Create a funnel using the [crm.category.add](./crm-category-add.md) method or find a suitable one among the existing funnels using the [crm.category.list](./crm-category-list.md) method.
3. Configure the stages of the new funnel using the [crm.status.*](../../status/index.md) methods: each funnel has its own reference of stages.
4. Link an item to the funnel: pass the funnel's `id` as `categoryId` in the [crm.item.add](../crm-item-add.md) or [crm.item.update](../crm-item-update.md) method.
5. If necessary, configure the card layout for this funnel using the [crm.item.details.configuration.*](../item-details-configuration/index.md) methods.

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

**Stages.** Each funnel defines its own reference of stages with a unique `ENTITY_ID`. Stages are handled by the [crm.status.*](../../status/index.md) methods. The identifiers of the references are returned by the [crm.status.entity.types](../../status/crm-status-entity-types.md) method.

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

## Continue Learning

- [{#T}](../index.md)
- [{#T}](../../status/index.md)
- [{#T}](../user-defined-object-types/index.md)