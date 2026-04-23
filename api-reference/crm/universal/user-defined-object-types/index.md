# Smart Processes: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A Smart Process is a versatile CRM entity that can be customized to meet the needs of a company. For each Smart Process, Bitrix24 creates a separate section in the CRM. In this section, you can configure Sales Funnels and stages, Automation rules, fields, and connections to other Bitrix24 entities.

> Quick Navigation: [All Methods and Events](#all-methods)
>
> User Documentation: [Smart Process Automation in CRM](https://helpdesk.bitrix24.com/open/19141012/)

## Working with a Smart Process

1. Create and configure the Smart Process — methods [crm.type.*](./index.md).
2. Set up Sales Funnels and stages — [crm.category.*](../category/index.md) for funnels and [crm.status.*](../../status/index.md) for stages.
3. Add custom fields — [userfieldconfig.*](../userfieldconfig/index.md).
4. Configure the item detail form — [crm.item.details.configuration.*](../item-details-configuration/index.md).
5. Create the first items within the Smart Process — [crm.item.*](../index.md).

The Smart Process can be transferred from the CRM section to the Automation section via [digital workplaces](../../automated-solution/index.md).

## Connections with Other Entities

**CRM Entities.** A Smart Process can be [linked](./crm-type-add.md#relations) to leads, deals, and other CRM entities. The linked entity will be accessible through the field `parentId{ID}`, where `{ID}` is the numeric identifier of the CRM entity.

**Client.** This field in the Smart Process detail form consists of the associated company and contacts. There is one company in the field; change the linked company through the `companyId` field. There can be multiple contacts in the "Client" field. Interaction with contacts is managed through the `contactIds` field — pass an array of contact IDs in this field. Enable the field with the `isClientEnabled` option in the [crm.type.add](./crm-type-add.md) or [crm.type.update](./crm-type-update.md) methods.

**Your Company Details.** Specify your company ID in the `mycompanyId` field so that its details are automatically used in documents. You can obtain your company ID using the [crm.item.list](../crm-item-list.md) method for the company object with a filter on the `isMyCompany` field. Enable the field with the `isMycompanyEnabled` option in the [crm.type.add](./crm-type-add.md) or [crm.type.update](./crm-type-update.md) methods.

**Products.** To add, modify, or delete product items in the Smart Process, use the [crm.item.productrow.*](../product-rows/index.md) methods. Enable the products tab and the "Amount and Currency" field with the `isLinkWithProductsEnabled` option in the [crm.type.add](./crm-type-add.md) or [crm.type.update](./crm-type-update.md) methods.

**Users.** The Smart Process is linked to users by numeric identifiers in the fields:
- `createdBy` — created by,
- `updatedBy` — updated by,
- `movedBy` — who changed the stage,
- `assignedById` — responsible for the item,
- `observers` — observers. Enable the field with the `isObserversEnabled` option in the [crm.type.add](./crm-type-add.md) or [crm.type.update](./crm-type-update.md) methods.

You can obtain the user ID and data using the [user.get](../../../user/user-get.md) method.

**Documents.** To create a document from a template, upload a new template for the Smart Process, or configure the document numbering, use the [crm.documentgenerator.*](../../document-generator/index.md) methods. Enable document handling in the Smart Process with the `isDocumentsEnabled` option in the [crm.type.add](./crm-type-add.md) or [crm.type.update](./crm-type-update.md) methods.

**Tasks.** Smart Process items can be linked to tasks. Use the [tasks.task.*](../../../tasks/index.md) methods to work with tasks. To make the linking option available, enable and configure the `isUseInUserfieldEnabled` option in the [crm.type.add](./crm-type-add.md) or [crm.type.update](./crm-type-update.md) methods.

**Calendar Events.** Smart Process items can be linked to calendar events. Use the [calendar.event.*](../../../calendar/calendar-event/index.md) methods to work with the calendar. To make the linking option available, enable and configure the `isUseInUserfieldEnabled` option in the [crm.type.add](./crm-type-add.md) or [crm.type.update](./crm-type-update.md) methods.

{% note tip "User Documentation" %}

- [How to Attach a Task to a Smart Process](../../../../tutorials/tasks/how-to-connect-task-to-spa.md)
- [How to Create a Custom Field in a Smart Process](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-user-field-to-spa.md)
- [How to Add a Comment to the Smart Process Timeline](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-comment-to-spa.md)
- [How to Create a New Funnel with Stages in a Smart Process](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-category-to-spa.md)  

{% endnote %}

## Smart Process Item Detail Form

The main workspace in a Smart Process is the "General" tab of the detail form. It consists of two parts:

- The left part contains fields with information. If the system fields are insufficient, you can create your own custom fields. Fields allow you to store information in various data formats: string, number, link, address, and others. To create, modify, retrieve, or delete custom fields in the Smart Process, use the group of methods [userfieldconfig.*](../userfieldconfig/index.md).

- The right part contains the Smart Process timeline. Here, you can create, edit, filter, and delete CRM activities — a group of methods [crm.activity.*](../../timeline/activities/index), and timeline records — a group of methods [crm.timeline.*](../../timeline/index).

You can manage the parameters of the Smart Process detail form through the group of methods [crm.item.details.configuration.*](../item-details-configuration/index.md).

{% note tip "User Documentation" %}

- [CRM item form features and settings](https://helpdesk.bitrix24.com/open/22879716/)
- [Standard fields in CRM](https://helpdesk.bitrix24.com/open/18529390/)
- [Custom fields in CRM](https://helpdesk.bitrix24.com/open/22067852/)
- [Timeline in CRM item form](https://helpdesk.bitrix24.com/open/25816403/)

{% endnote %}

## Widgets

You can embed an application into the Smart Process detail form. This allows you to use the application without leaving the item detail form.

There are two embedding scenarios:

- Use special [embedding locations](../../../widgets/crm/index.md). For example, by creating your own tab.
  
- Create a [custom field](../../../../tutorials/crm/crm-widgets/widget-as-field-in-lead-page.md) where the interface of your application will be loaded.
  
### Embedding Locations for Smart Processes

Replace `XXX` with the numeric identifier of the specific Smart Process, for example, `CRM_DYNAMIC_183_DOCUMENTGENERATOR_BUTTON`.

- [`CRM_DYNAMIC_XXX_DETAIL_TAB`](../../../widgets/crm/detail-tab.md) — tab in the detail form of the CRM item

- [`CRM_DYNAMIC_XXX_DETAIL_ACTIVITY`](../../../widgets/crm/detail-activity.md) — button above the item detail timeline

- [`CRM_DYNAMIC_XXX_DETAIL_TOOLBAR`](../../../widgets/crm/detail-toolbar.md) — item in the dropdown menu of the top button in the detail form

- [`CRM_DYNAMIC_XXX_DOCUMENTGENERATOR_BUTTON`](../../../widgets/crm/document-generator-button.md) — item in the dropdown menu of the document generator

- [`CRM_DYNAMIC_XXX_LIST_MENU`](../../../widgets/crm/index.md) — item in the context menu in the list of items

- [`CRM_DYNAMIC_XXX_LIST_TOOLBAR`](../../../widgets/crm/list-toolbar.md) — item in the dropdown menu above the list of items

- [`CRM_DYNAMIC_XXX_ACTIVITY_TIMELINE_MENU`](../../../widgets/crm/activity-timeline-menu.md) — item in the context menu of the activity in the item detail form

- [`CRM_DYNAMIC_XXX_ROBOT_DESIGNER_TOOLBAR`](../../../widgets/crm/robot-designer-toolbar.md) — item in the dropdown menu of the top button of the Automation rule designer

{% note tip "Typical Use-Cases and Scenarios" %}

- [Widget Embedding Mechanism](../../../widgets/index.md)
- [Embed a Widget in the CRM Detail Form](../../../../tutorials/crm/crm-widgets/widget-as-detail-tab.md)

{% endnote %}

## Smart Process Identifiers {#id}

Each Smart Process has four types of identifiers. Use these identifiers to apply a method to a specific Smart Process.

1. Numeric identifier of type `130`. Obtain it using the [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md) `ID` or [crm.type.list](./crm-type-list.md) `entityTypeId`.

1. Symbolic code of type `DYNAMIC_130` — [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md) `SYMBOL_CODE`.

2. Short symbolic code of type `T82` — [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md) `SYMBOL_CODE_SHORT`.

3. Custom field object type `CRM_13` — [crm.type.list](./crm-type-list.md). Substitute the `id` from the method result into the formula `CRM_ + {ID}`.

## Overview of Methods and Events {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

### Smart Processes

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [crm.type.add](./crm-type-add.md) | Creates a new Smart Process ||
    || [crm.type.update](./crm-type-update.md) | Updates the Smart Process ||
    || [crm.type.get](./crm-type-get.md) | Returns the Smart Process by id ||
    || [crm.type.getByEntityTypeId](./crm-type-get-by-entity-type-id.md) | Returns the Smart Process by entityTypeId ||
    || [crm.type.list](./crm-type-list.md) | Returns a list of Smart Processes ||
    || [crm.type.delete](./crm-type-delete.md) | Deletes the Smart Process ||
    || [crm.type.fields](./crm-type-fields.md) | Returns the fields of the Smart Process ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [onCrmTypeAdd](../events/type/on-crm-type-add.md) | When a Smart Process is created ||
    || [onCrmTypeUpdate](../events/type/on-crm-type-update.md) | When a Smart Process is updated ||
    || [onCrmTypeDelete](../events/type/on-crm-type-delete.md) | When a Smart Process is deleted ||
    |#

{% endlist %}

### Items

The CRM object identifier **entityTypeId** — [numeric identifier type](#id), for example, `128`.

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [crm.item.add](../crm-item-add.md) | Creates a new item ||
    || [crm.item.update](../crm-item-update.md) | Updates the item ||
    || [crm.item.get](../crm-item-get.md) | Returns the item by Id ||
    || [crm.item.list](../crm-item-list.md) | Returns a list of items by filter ||
    || [crm.item.delete](../crm-item-delete.md) | Deletes the item ||
    || [crm.item.fields](../crm-item-fields.md) | Returns the fields of the item ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [onCrmDynamicItemAdd](../events/on-crm-dynamic-item-add.md) | When a Smart Process item is created ||
    || [onCrmDynamicItemDelete](../events/on-crm-dynamic-item-delete.md) | When a Smart Process item is deleted ||
    || [onCrmDynamicItemUpdate](../events/on-crm-dynamic-item-update.md) | When a Smart Process item is updated ||
    |#

{% endlist %}

### Sales Funnels

The CRM object identifier **entityTypeId** — [numeric identifier type](#id), for example, `128`.

#|
|| **Method** | **Description** ||
|| [crm.category.add](../category/crm-category-add.md) | Creates a new Sales Funnel ||
|| [crm.category.update](../category/crm-category-update.md) | Updates the Sales Funnel ||
|| [crm.category.get](../category/crm-category-get.md) | Returns the Sales Funnel by Id ||
|| [crm.category.list](../category/crm-category-list.md) | Returns a list of Sales Funnels ||
|| [crm.category.delete](../category/crm-category-delete.md) | Deletes the Sales Funnel ||
|| [crm.category.fields](../category/crm-category-fields.md) | Returns the fields of the Sales Funnel ||
|#

### Custom Fields

The CRM object identifier **entityId** — [custom field object type](#id), for example, `CRM_1`.

#|
|| **Method** | **Description** ||
|| [userfieldconfig.add](../userfieldconfig/userfieldconfig-add.md) | Creates a custom field ||
|| [userfieldconfig.update](../userfieldconfig/userfieldconfig-update.md) | Modifies the field settings ||
|| [userfieldconfig.get](../userfieldconfig/userfieldconfig-get.md) | Returns the settings of the custom field by identifier ||
|| [userfieldconfig.getTypes](../userfieldconfig/userfieldconfig-get-types.md) | Returns the set of available custom field types for the module ||
|| [userfieldconfig.list](../userfieldconfig/userfieldconfig-list.md) | Returns a list of custom field settings ||
|| [userfieldconfig.delete](../userfieldconfig/userfieldconfig-delete.md) | Deletes the custom field ||
|#

### Managing Detail Form Settings

The CRM object identifier **entityTypeId** — [numeric identifier type](#id), for example, `128`.

#|
|| **Method** | **Description** ||
|| [crm.item.details.configuration.forceCommonScopeForAll](../item-details-configuration/crm-item-details-configuration-forceCommonScopeForAll.md) | Sets a common detail form for all users ||
|| [crm.item.details.configuration.get](../item-details-configuration/crm-item-details-configuration-get.md) | Retrieves the parameters of the item detail form ||
|| [crm.item.details.configuration.reset](../item-details-configuration/crm-item-details-configuration-reset.md) | Resets the parameters of the item detail form ||
|| [crm.item.details.configuration.set](../item-details-configuration/crm-item-details-configuration-set.md) | Sets the parameters of the item detail form ||
|#

### Product Items

The CRM object identifier **ownerType** — [short symbolic code type](#id), for example, `T80`.

#|
|| **Method** | **Description** ||
|| [crm.item.productrow.add](../product-rows/crm-item-productrow-add.md) | Adds a product item ||
|| [crm.item.productrow.update](../product-rows/crm-item-productrow-update.md) | Updates a product item ||
|| [crm.item.productrow.get](../product-rows/crm-item-productrow-get.md) | Retrieves information about a product item by id ||
|| [crm.item.productrow.set](../product-rows/crm-item-productrow-set.md) | Links a product item to a CRM object ||
|| [crm.item.productrow.list](../product-rows/crm-item-productrow-list.md) | Retrieves a list of product items ||
|| [crm.item.productrow.getAvailableForPayment](../product-rows/crm-item-productrow-get-available-for-payment.md) | Retrieves a list of unpaid products ||
|| [crm.item.productrow.delete](../product-rows/crm-item-productrow-delete.md) | Deletes a product item ||
|| [crm.item.productrow.fields](../product-rows/crm-item-productrow-fields.md) | Retrieves a list of product item fields ||
|#