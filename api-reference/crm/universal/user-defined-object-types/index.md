# Smart Processes: Overview of Methods

A smart process is a versatile CRM object that can be customized to meet the needs of a company. For each smart process, Bitrix24 creates a separate section in the CRM. In this section, you can configure funnels and stages, Automation rules, fields, and connections with other Bitrix24 objects.

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [Smart Processes in CRM](https://helpdesk.bitrix24.com/open/19141012/)

## Working with a Smart Process

1. Create and configure the smart process — methods [crm.type.*](./index.md).
2. Set up funnels and stages — [crm.category.*](../category/index.md) for funnels and [crm.status.*](../../status/index.md) for stages.
3. Add custom fields — [userfieldconfig.*](../userfieldconfig/userfieldconfig/index.md).
4. Configure the item detail form — [crm.item.details.configuration.*](../item-details-configuration/index.md).
5. Create the first items within the smart process — [crm.item.*](../index.md).

The smart process can be transferred from the CRM section to the Automation section via [digital workplaces](../../automated-solution/index.md).

## Connections with Other Objects

**CRM Objects.** A smart process can be [linked](./crm-type-add.md#relations) to leads, deals, and other CRM objects. The linked object will be accessible through the field `parentId{ID}`, where `{ID}` is the numeric identifier of the CRM object.

**Client.** A field in the smart process detail form that consists of the associated company and contacts. There is one company in the field; change the linked company through the `companyId` field. There can be multiple contacts in the "Client" field. Interaction with contacts is conducted through the `contactIds` field — pass an array of contact IDs into the field. Enable the field with the option `isClientEnabled` in the method [crm.type.add](./crm-type-add.md) or [crm.type.update](./crm-type-update.md).

**Your Company Details.** Specify your company ID in the `mycompanyId` field so that its details are automatically used in documents. You can obtain your company ID using the method [crm.item.list](../crm-item-list.md) for the company object with a filter on the `isMyCompany` field. Enable the field with the option `isMycompanyEnabled` in the method [crm.type.add](./crm-type-add.md) or [crm.type.update](./crm-type-update.md).

**Products.** To add, modify, or delete product items in the smart process, use the methods [crm.item.productrow.*](../product-rows/index.md). Enable the products tab and the "Amount and Currency" field with the option `isLinkWithProductsEnabled` in the method [crm.type.add](./crm-type-add.md) or [crm.type.update](./crm-type-update.md).

**Users.** The smart process is linked to users by numeric identifiers in the fields:
- `createdBy` — who created it,
- `updatedBy` — who updated it,
- `movedBy` — who changed the stage,
- `assignedById` — responsible for the item,
- `observers` — observers. Enable the field with the option `isObserversEnabled` in the method [crm.type.add](./crm-type-add.md) or [crm.type.update](./crm-type-update.md).

You can obtain the identifier and data of a user using the method [user.get](../../../user/user-get.md).

**Documents.** To create a document from a template, upload a new template for the smart process, or configure the numbering for documents, use the methods [crm.documentgenerator.*](../../document-generator/index.md). Enable document handling in the smart process with the option `isDocumentsEnabled` in the method [crm.type.add](./crm-type-add.md) or [crm.type.update](./crm-type-update.md).

**Tasks.** Smart process items can be linked to tasks. To work with tasks, use the methods [tasks.task.*](../../../tasks/index.md). To make the linking option available, enable and configure the option `isUseInUserfieldEnabled` in the method [crm.type.add](./crm-type-add.md) or [crm.type.update](./crm-type-update.md).

**Calendar Events.** Smart process items can be linked to calendar events. To work with the calendar, use the methods [calendar.event.*](../../../calendar/calendar-event/index.md). To make the linking option available, enable and configure the option `isUseInUserfieldEnabled` in the method [crm.type.add](./crm-type-add.md) or [crm.type.update](./crm-type-update.md).

{% note tip "User Documentation" %}

- [How to attach a task to a smart process](../../../../tutorials/tasks/how-to-connect-task-to-spa.md)
- [How to create a custom field in a smart process](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-user-field-to-spa.md)
- [How to add a comment to the smart process timeline](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-comment-to-spa.md)
- [How to create a new funnel with stages in a smart process](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-category-to-spa.md)  

{% endnote %}

## Smart Process Item Detail Form

The main workspace in the smart process is the "General" tab of the detail form. It consists of two parts:

- the left part, which contains fields with information. If the system fields are insufficient, you can create your own custom fields. Fields allow you to store information in various data formats: string, number, link, address, and others. To create, modify, retrieve, or delete custom fields in the smart process, use the group of methods [userfieldconfig.*](../userfieldconfig/userfieldconfig/index.md).

- the right part, which contains the smart process timeline. In it, you can create, edit, filter, and delete CRM activities — a group of methods [crm.activity.*](../../timeline/activities/index), and timeline records — a group of methods [crm.timeline.*](../../timeline/index).

You can manage the parameters of the smart process detail form through the group of methods [crm.item.details.configuration.*](../item-details-configuration/index.md).

{% note tip "User Documentation" %}

- [CRM Detail Form: Features and Settings](https://helpdesk.bitrix24.com/open/22879716/)
- [System Fields in CRM](https://helpdesk.bitrix24.com/open/18529390/)
- [Custom Fields in CRM](https://helpdesk.bitrix24.com/open/22067852/)
- [Timeline in CRM Item](https://helpdesk.bitrix24.com/open/16767378/)

{% endnote %}

## Widgets

You can embed an application into the smart process detail form. Thanks to embedding, you can use the application without leaving the item detail form.

There are two embedding scenarios:

- use special [embedding locations](../../../widgets/crm/index.md). For example, by creating your own tab.
  
- create a [custom field](../../../../tutorials/crm/crm-widgets/widget-as-field-in-lead-page.md), where the interface of your application will be loaded.
  
### Smart Process Embedding Locations

Replace `XXX` with the numeric identifier of a specific smart process, for example `CRM_DYNAMIC_183_DOCUMENTGENERATOR_BUTTON`.

- [`CRM_DYNAMIC_XXX_DETAIL_TAB`](../../../widgets/crm/detail-tab.md) — tab in the detailed CRM item form

- [`CRM_DYNAMIC_XXX_DETAIL_ACTIVITY`](../../../widgets/crm/detail-activity.md) — button above the item detail timeline

- [`CRM_DYNAMIC_XXX_DETAIL_TOOLBAR`](../../../widgets/crm/detail-toolbar.md) — dropdown menu item in the upper button of the detail form

- [`CRM_DYNAMIC_XXX_DOCUMENTGENERATOR_BUTTON`](../../../widgets/crm/document-generator-button.md) — dropdown menu item for the document generator

- [`CRM_DYNAMIC_XXX_LIST_MENU`](../../../widgets/crm/index.md) — context menu item in the list of items

- [`CRM_DYNAMIC_XXX_LIST_TOOLBAR`](../../../widgets/crm/list-toolbar.md) — dropdown menu item above the list of items

- [`CRM_DYNAMIC_XXX_ACTIVITY_TIMELINE_MENU`](../../../widgets/crm/activity-timeline-menu.md) — context menu item for an activity in the item detail form

- [`CRM_DYNAMIC_XXX_ROBOT_DESIGNER_TOOLBAR`](../../../widgets/crm/robot-designer-toolbar.md) — dropdown menu item in the upper button of the robot designer

{% note tip "Typical use-cases and scenarios" %}

- [Widget Embedding Mechanism](../../../widgets/index.md)
- [Embed a widget in the CRM detail form](../../../../tutorials/crm/crm-widgets/widget-as-detail-tab.md)

{% endnote %}

## Smart Process Identifiers {#id}

Each smart process has four types of identifiers. Use the identifiers to apply a method to a specific smart process.

1. Numeric identifier of type `130`. Obtain it using the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md) `ID` or [crm.type.list](./crm-type-list.md) `entityTypeId`.

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
    || [crm.type.add](./crm-type-add.md) | Creates a new smart process ||
    || [crm.type.update](./crm-type-update.md) | Updates the smart process ||
    || [crm.type.get](./crm-type-get.md) | Returns the smart process by id ||
    || [crm.type.getByEntityTypeId](./crm-type-get-by-entity-type-id.md) | Returns the smart process by entityTypeId ||
    || [crm.type.list](./crm-type-list.md) | Returns a list of smart processes ||
    || [crm.type.delete](./crm-type-delete.md) | Deletes the smart process ||
    || [crm.type.fields](./crm-type-fields.md) | Returns the fields of the smart process ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [onCrmTypeAdd](../events/type/on-crm-type-add.md) | When a smart process is created ||
    || [onCrmTypeUpdate](../events/type/on-crm-type-update.md) | When a smart process is updated ||
    || [onCrmTypeDelete](../events/type/on-crm-type-delete.md) | When a smart process is deleted ||
    |#

{% endlist %}

### Items

The CRM object identifier **entityTypeId** — [numeric identifier type](#id), for example `128`.

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
    || [onCrmDynamicItemAdd](../events/on-crm-dynamic-item-add.md) | When a smart process item is created ||
    || [onCrmDynamicItemDelete](../events/on-crm-dynamic-item-delete.md) | When a smart process item is deleted ||
    || [onCrmDynamicItemUpdate](../events/on-crm-dynamic-item-update.md) | When a smart process item is modified ||
    |#

{% endlist %}

### Funnels

The CRM object identifier **entityTypeId** — [numeric identifier type](#id), for example `128`.

#|
|| **Method** | **Description** ||
|| [crm.category.add](../category/crm-category-add.md) | Creates a new funnel ||
|| [crm.category.update](../category/crm-category-update.md) | Updates the funnel ||
|| [crm.category.get](../category/crm-category-get.md) | Returns the funnel by Id ||
|| [crm.category.list](../category/crm-category-list.md) | Returns a list of funnels ||
|| [crm.category.delete](../category/crm-category-delete.md) | Deletes the funnel ||
|| [crm.category.fields](../category/crm-category-fields.md) | Returns the fields of the funnel ||
|#

### Custom Fields

The CRM object identifier **entityId** — [custom field object type](#id), for example `CRM_1`.

#|
|| **Method** | **Description** ||
|| [userfieldconfig.add](../userfieldconfig/userfieldconfig/userfieldconfig-add.md) | Creates a custom field ||
|| [userfieldconfig.update](../userfieldconfig/userfieldconfig/userfieldconfig-update.md) | Modifies field settings ||
|| [userfieldconfig.get](../userfieldconfig/userfieldconfig/userfieldconfig-get.md) | Returns custom field settings by identifier ||
|| [userfieldconfig.getTypes](../userfieldconfig/userfieldconfig/userfieldconfig-get-types.md) | Returns the set of available custom field types for the module ||
|| [userfieldconfig.list](../userfieldconfig/userfieldconfig/userfieldconfig-list.md) | Returns a list of custom field settings ||
|| [userfieldconfig.delete](../userfieldconfig/userfieldconfig/userfieldconfig-delete.md) | Deletes a custom field ||
|#

### Managing Detail Form Settings

The CRM object identifier **entityTypeId** — [numeric identifier type](#id), for example `128`.

#|
|| **Method** | **Description** ||
|| [crm.item.details.configuration.forceCommonScopeForAll](../item-details-configuration/crm-item-details-configuration-forceCommonScopeForAll.md) | Sets a common detail form for all users ||
|| [crm.item.details.configuration.get](../item-details-configuration/crm-item-details-configuration-get.md) | Retrieves the parameters of item detail forms ||
|| [crm.item.details.configuration.reset](../item-details-configuration/crm-item-details-configuration-reset.md) | Resets the parameters of item detail forms ||
|| [crm.item.details.configuration.set](../item-details-configuration/crm-item-details-configuration-set.md) | Sets the parameters of item detail forms ||
|#

### Product Items

The CRM object identifier **ownerType** — [short symbolic code type](#id), for example `T80`.

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