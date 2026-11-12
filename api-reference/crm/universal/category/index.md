# CRM Pipelines: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The methods `crm.category.*` manage pipelines. A pipeline is a separate branch of work with a CRM object that has its own set of stages and its own card settings.

Pipelines are used to separate work by departments or types of sales. They are most commonly configured for deals and SPAs.

{% note warning "DEPRECATED" %}

The development of the `crm.category.*` methods has been halted. For supported objects, use the [REST 3.0 pipeline methods](../../rest-v3/categories/index.md). For invoice, CRM document, and B2E document pipelines, keep using `crm.category.*` for now.

{% endnote %}

> Quick Navigation: [All Methods](#all-methods)
>
> User Documentation: [Sales pipelines](https://helpdesk.bitrix24.com/open/20739996/)

## Which Objects Support Pipelines

The object for which a pipeline is needed is specified by the `entityTypeId` parameter. Not all CRM objects support pipelines.

#|
|| **Object** | **`entityTypeId`** ||
|| Deal | `2` ||
|| Contact | `3` ||
|| Company | `4` ||
|| Invoice | `31` ||
|| CRM document | `36` ||
|| B2E document | `39` ||
|| SPA | from `128` ||
|#

The SPA identifier is returned by the [crm.type.list](../user-defined-object-types/crm-type-list.md) method, and the complete table of types is available in the [CRM object types reference](../../data-types.md#object_type). For an object without pipeline support, such as a lead, the `crm.category.*` methods will return the `ENTITY_TYPE_NOT_SUPPORTED` error.

## Getting Started

For objects supported by REST 3.0, start with the [REST 3.0 pipeline methods](../../rest-v3/categories/index.md). The steps below apply to `crm.category.*`.

1. Determine the `entityTypeId` of the object for which the pipeline is needed.
2. Create a pipeline using the [crm.category.add](./crm-category-add.md) method or find a suitable one among the existing pipelines using the [crm.category.list](./crm-category-list.md) method.
3. If necessary, configure the stages of the selected pipeline using the [crm.status.*](../../status/index.md) methods: each pipeline has its own reference of stages.
4. Link an item to the pipeline: pass the pipeline's `id` as `categoryId` in the [crm.item.add](../crm-item-add.md) or [crm.item.update](../crm-item-update.md) method.
5. If necessary, configure the card layout for this pipeline using the [crm.item.details.configuration.*](../item-details-configuration/index.md) methods.

{% note tip "Typical use-cases and scenarios" %}

- [How to Create a New Pipeline with Stages in an SPA](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-category-to-spa.md)
- [How to Filter Items by Stage Name](../../../../tutorials/crm/how-to-get-lists/how-to-get-elements-by-stage-filter.md)
- [How to Create a Vendor](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-contractor.md)
- [How to Get a List of Vendors](../../../../tutorials/crm/how-to-get-lists/how-to-get-contractors.md)

{% endnote %}

## Important Considerations

**Pipeline Fields.** The main fields are `id` (identifier), `name` (name), `sort` (sort order), `entityTypeId` (CRM object type), and `isDefault` (default pipeline flag). The [crm.category.fields](./crm-category-fields.md) method returns the full set of fields for the selected object type.

**Access Permissions.** The [crm.category.list](./crm-category-list.md) method is available to any user but only returns those pipelines for which the user has read access.

**Default Pipeline.** The behavior of `isDefault` depends on the object type: in deals, the field cannot be changed. For details, see the descriptions of [creating](./crm-category-add.md#parametr-fields) and [updating](./crm-category-update.md#parametr-fields) a pipeline.

**Deleting a Pipeline.** A pipeline that contains items cannot be deleted. Other restrictions depend on the object type and are described in the [crm.category.delete](./crm-category-delete.md) method.

## Relationship with Other Entities

**SPAs.** The `crm.category.*` methods are available even when pipelines are hidden in the interface: the `isCategoriesEnabled` option controls only their display. You can check the setting and retrieve the `entityTypeId` of the SPA using the [crm.type.list](../user-defined-object-types/crm-type-list.md) method.

**Stages.** Each pipeline defines its own reference of stages with a unique `ENTITY_ID`. Stages are handled by the [crm.status.*](../../status/index.md) methods. The identifiers of the references are returned by the [crm.status.entity.types](../../status/crm-status-entity-types.md) method.

**Deal Cards.** The settings of deal cards depend on the pipeline. To configure a card for a specific pipeline, pass the pipeline's `id` as `dealCategoryId` in the [crm.deal.details.configuration.get](../../deals/custom-form/crm-deal-details-configuration-get.md) and [crm.deal.details.configuration.set](../../deals/custom-form/crm-deal-details-configuration-set.md) methods.

## Continue Learning

- [{#T}](../index.md)
- [{#T}](../../status/index.md)
- [{#T}](../user-defined-object-types/index.md)

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: depends on the method
>
> To create, update, and delete pipelines: a user with the permission to manage pipelines. For an SPA in a digital workplace, the permission is checked within that workplace.

#| 
|| **Method** | **Description** ||
|| [crm.category.add](./crm-category-add.md) | Creates a new pipeline ||
|| [crm.category.update](./crm-category-update.md) | Updates a pipeline ||
|| [crm.category.get](./crm-category-get.md) | Returns a pipeline by `id` ||
|| [crm.category.list](./crm-category-list.md) | Returns a list of pipelines ||
|| [crm.category.delete](./crm-category-delete.md) | Deletes a pipeline ||
|| [crm.category.fields](./crm-category-fields.md) | Returns the description of pipeline fields ||
|#