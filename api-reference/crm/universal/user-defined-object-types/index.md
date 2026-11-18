# SPAs: Overview of Methods and Events

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

An SPA is a versatile CRM object that can be customized to meet the needs of a company. For each SPA, Bitrix24 creates a separate section in the CRM. In this section, you can configure Sales Funnels and stages, Automation rules, fields, and connections to other Bitrix24 entities.

> Quick Navigation: [All Methods and Events](#all-methods)
>
> User Documentation: [Smart Process Automation in CRM](https://helpdesk.bitrix24.com/open/19141012/)

## Working with an SPA

1. Create and configure the SPA — methods [crm.type.*](#all-methods).
2. Set up Sales Funnels and stages — [crm.category.*](../category/index.md) for funnels and [crm.status.*](../../status/index.md) for stages.
3. Add custom fields — [userfieldconfig.*](../userfieldconfig/index.md).
4. Configure the item detail form — [crm.item.details.configuration.*](../item-details-configuration/index.md).
5. Create the first items within the SPA — [crm.item.*](../index.md).

The SPA can be transferred from the CRM section to the Automation section via [automated solutions](../../automated-solution/index.md).

## Connections with Other Entities

**CRM Entities.** An SPA can be [linked](./crm-type-add.md#relations) to leads, deals, and other CRM entities. Manage links between items with the [crm.item.*](../index.md) methods.

**Client.** An SPA item can be linked to one company and multiple contacts. Manage the client with the [crm.item.*](../index.md) methods.

**Your Company Details.** Link an item to your company to use its details in documents. You can find your company using the [crm.item.list](../crm-item-list.md) method for companies with a filter on the `isMyCompany` field.

**Products.** To add, modify, or delete product items in the SPA, use the [crm.item.productrow.*](../product-rows/index.md) methods.

**Users.** An item stores references to its creator, the user who last modified it, the responsible person, and observers. The fields are described in [crm.item.fields](../crm-item-fields.md); you can retrieve user IDs and data using the [user.get](../../../user/user-get.md) method.

**Documents.** To create a document from a template, upload a new template for the SPA, or configure the document numbering, use the [crm.documentgenerator.*](../../document-generator/index.md) methods.

**Tasks.** SPA items can be linked to tasks. Use the [tasks.task.*](../../../tasks/index.md) methods to work with tasks.

**Calendar Events.** SPA items can be linked to calendar events. Use the [calendar.event.*](../../../calendar/calendar-event/index.md) methods to work with the calendar.

These features are enabled by options in the [crm.type.add](./crm-type-add.md) and [crm.type.update](./crm-type-update.md) methods.

#|
|| **Feature** | **Option in crm.type.add / crm.type.update** | **Methods** ||
|| Client: company and contacts | `isClientEnabled` | [crm.item.*](../index.md) ||
|| Your company details | `isMycompanyEnabled` | [crm.item.*](../index.md) ||
|| Products, amount, and currency | `isLinkWithProductsEnabled` | [crm.item.productrow.*](../product-rows/index.md), [crm.item.*](../index.md) ||
|| Observers | `isObserversEnabled` | [crm.item.*](../index.md) ||
|| Documents | `isDocumentsEnabled` | [crm.documentgenerator.*](../../document-generator/index.md) ||
|| Linking to tasks and calendar events | `isUseInUserfieldEnabled` and the [linkedUserFields](./crm-type-add.md#linkedUserFields) setting | [tasks.task.*](../../../tasks/index.md), [calendar.event.*](../../../calendar/calendar-event/index.md) ||
|#

{% note tip "User Documentation" %}

- [How to Attach a Task to a Smart Process](../../../../tutorials/tasks/how-to-connect-task-to-spa.md)
- [How to Create a Custom Field in a Smart Process](../../../../tutorials/field-types/how-to-add-user-field-to-spa.md)
- [How to Add a Comment to the Smart Process Timeline](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-comment-to-spa.md)
- [How to Create a New Funnel with Stages in a Smart Process](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-category-to-spa.md)

{% endnote %}

## SPA Item Detail Form

The main workspace in an SPA is the "General" tab of the detail form. It consists of two parts:

- The left part contains fields with information. If the system fields are insufficient, you can create your own custom fields. Fields allow you to store information in various data formats: string, number, link, address, and others. To create, modify, retrieve, or delete custom fields in the SPA, use the group of methods [userfieldconfig.*](../userfieldconfig/index.md).

- The right part contains the SPA timeline. Here, you can create, edit, filter, and delete CRM activities — a group of methods [crm.activity.*](../../timeline/activities/index.md), and timeline records — a group of methods [crm.timeline.*](../../timeline/index.md).

You can manage the parameters of the SPA detail form through the group of methods [crm.item.details.configuration.*](../item-details-configuration/index.md).

{% note tip "User Documentation" %}

- [CRM item form features and settings](https://helpdesk.bitrix24.com/open/22879716/)
- [Standard fields in CRM](https://helpdesk.bitrix24.com/open/18529390/)
- [Custom fields in CRM](https://helpdesk.bitrix24.com/open/22067852/)
- [Timeline in CRM item form](https://helpdesk.bitrix24.com/open/25816403/)

{% endnote %}

## Widgets

You can embed an application into the SPA detail form. This allows you to use the application without leaving the item detail form.

There are two embedding scenarios:

- Use special [embedding locations](../../../widgets/crm/index.md). For example, by creating your own tab.

- Create a [custom field](../../../../tutorials/crm/crm-widgets/widget-as-field-in-lead-page.md) where the interface of your application will be loaded.

### Embedding Locations for SPAs

Replace `XXX` with the `entityTypeId` of the SPA, for example, `CRM_DYNAMIC_183_DOCUMENTGENERATOR_BUTTON`.

- [`CRM_DYNAMIC_XXX_DETAIL_TAB`](../../../widgets/crm/detail-tab.md) — tab in the detail form of the CRM item

- [`CRM_DYNAMIC_XXX_DETAIL_ACTIVITY`](../../../widgets/crm/detail-activity.md) — button above the item detail timeline

- [`CRM_DYNAMIC_XXX_DETAIL_TOOLBAR`](../../../widgets/crm/detail-toolbar.md) — item in the dropdown menu of the top button in the detail form

- [`CRM_DYNAMIC_XXX_DOCUMENTGENERATOR_BUTTON`](../../../widgets/crm/document-generator-button.md) — item in the dropdown menu of the document generator

- [`CRM_DYNAMIC_XXX_LIST_MENU`](../../../widgets/crm/list-menu.md) — item in the context menu in the list of items

- [`CRM_DYNAMIC_XXX_LIST_TOOLBAR`](../../../widgets/crm/list-toolbar.md) — item in the dropdown menu above the list of items

- [`CRM_DYNAMIC_XXX_ACTIVITY_TIMELINE_MENU`](../../../widgets/crm/activity-timeline-menu.md) — item in the context menu of the activity in the item detail form

- [`CRM_DYNAMIC_XXX_ROBOT_DESIGNER_TOOLBAR`](../../../widgets/crm/robot-designer-toolbar.md) — item in the dropdown menu of the top button of the Automation rule designer

{% note tip "Typical Use-Cases and Scenarios" %}

- [Widget Embedding Mechanism](../../../widgets/index.md)
- [Embed a Widget in the CRM Detail Form](../../../../tutorials/crm/crm-widgets/widget-as-detail-tab.md)

{% endnote %}

## SPA Identifiers {#id}

Each SPA has four types of identifiers. Use these identifiers to apply a method to a specific SPA.

In the table, all examples refer to the same SPA: it has `id = 13` and `entityTypeId = 156`.

#|
|| **Identifier** | **Example** | **Where It Is Used** | **How to Obtain It** ||
|| Numeric identifier of the type | `156` | The `entityTypeId` parameter in the `crm.item.*`, `crm.category.*`, and `crm.item.details.configuration.*` methods | [crm.type.list](./crm-type-list.md), the `entityTypeId` key, or [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md), the `ID` key ||
|| Symbolic code of the type | `DYNAMIC_156` | Stage reference identifiers such as `DYNAMIC_156_STAGE_52` and object codes in custom field lists | [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md), the `SYMBOL_CODE` key ||
|| Short symbolic code of the type | `T9c` | The `ownerType` parameter in the `crm.item.productrow.*` methods | [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md), the `SYMBOL_CODE_SHORT` key ||
|| Custom field object type | `CRM_13` | The `entityId` parameter in the `userfieldconfig.*` methods | [crm.type.list](./crm-type-list.md), the `id` key substituted into the formula `CRM_{id}` ||
|#

{% note warning "" %}

`id` and `entityTypeId` are different values, and one is not calculated from the other.

{% endnote %}

## Overview of Methods and Events {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md); for the `userfieldconfig.*` methods — [`userfieldconfig`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

### SPAs

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [crm.type.add](./crm-type-add.md) | Creates a new SPA ||
    || [crm.type.update](./crm-type-update.md) | Updates the SPA ||
    || [crm.type.get](./crm-type-get.md) | Returns the SPA by id ||
    || [crm.type.getByEntityTypeId](./crm-type-get-by-entity-type-id.md) | Returns the SPA by entityTypeId ||
    || [crm.type.list](./crm-type-list.md) | Returns a list of SPAs ||
    || [crm.type.delete](./crm-type-delete.md) | Deletes the SPA ||
    || [crm.type.fields](./crm-type-fields.md) | Returns the fields of the SPA ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [onCrmTypeAdd](../events/type/on-crm-type-add.md) | When an SPA is created ||
    || [onCrmTypeUpdate](../events/type/on-crm-type-update.md) | When an SPA is updated ||
    || [onCrmTypeDelete](../events/type/on-crm-type-delete.md) | When an SPA is deleted ||
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
    || [onCrmDynamicItemAdd](../events/on-crm-dynamic-item-add.md) | When an SPA item is created ||
    || [onCrmDynamicItemDelete](../events/on-crm-dynamic-item-delete.md) | When an SPA item is deleted ||
    || [onCrmDynamicItemUpdate](../events/on-crm-dynamic-item-update.md) | When an SPA item is updated ||
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

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [userfieldconfig.add](../userfieldconfig/userfieldconfig-add.md) | Creates a custom field ||
    || [userfieldconfig.update](../userfieldconfig/userfieldconfig-update.md) | Modifies the field settings ||
    || [userfieldconfig.get](../userfieldconfig/userfieldconfig-get.md) | Returns the settings of the custom field by identifier ||
    || [userfieldconfig.getTypes](../userfieldconfig/userfieldconfig-get-types.md) | Returns the set of available custom field types for the module ||
    || [userfieldconfig.list](../userfieldconfig/userfieldconfig-list.md) | Returns a list of custom field settings ||
    || [userfieldconfig.delete](../userfieldconfig/userfieldconfig-delete.md) | Deletes the custom field ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [onCrmTypeUserFieldAdd](../userfieldconfig/events/on-crm-type-user-field-add.md) | When a custom field of an SPA is added ||
    || [onCrmTypeUserFieldUpdate](../userfieldconfig/events/on-crm-type-user-field-update.md) | When the settings of a custom field of an SPA are changed ||
    || [onCrmTypeUserFieldDelete](../userfieldconfig/events/on-crm-type-user-field-delete.md) | When a custom field of an SPA is deleted ||
    || [onCrmTypeUserFieldSetEnumValues](../userfieldconfig/events/on-crm-type-user-field-set-enum-values.md) | When the set of values of a list-type custom field is changed ||
    |#

{% endlist %}

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
|| [crm.item.productrow.set](../product-rows/crm-item-productrow-set.md) | Saves the set of product items of a CRM object ||
|| [crm.item.productrow.list](../product-rows/crm-item-productrow-list.md) | Retrieves a list of product items ||
|| [crm.item.productrow.getAvailableForPayment](../product-rows/crm-item-productrow-get-available-for-payment.md) | Retrieves product items with no payment issued ||
|| [crm.item.productrow.delete](../product-rows/crm-item-productrow-delete.md) | Deletes a product item ||
|| [crm.item.productrow.fields](../product-rows/crm-item-productrow-fields.md) | Retrieves a list of product item fields ||
|#

## Continue Learning

- [{#T}](../index.md)
- [{#T}](../category/index.md)
- [{#T}](../events/type/index.md)