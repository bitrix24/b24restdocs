# Deals in CRM: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A deal is one of the key objects in CRM, where you can:

* manage the sales process of a product or service, including tracking stages and accepting online payments
* engage in dialogue with the client: calls, e-mails, chats in open channels
* view the history of interactions: activities, timeline records

{% note warning "" %}

Development of the `crm.deal.*` methods has been discontinued. For new development, use the universal methods `crm.item.*` with `entityTypeId = 2`. The contact linking methods `crm.deal.contact.*`, the recurring deal methods `crm.deal.recurring.*`, and the custom field methods `crm.deal.userfield.*` continue to work.

{% endnote %}

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [deals in Bitrix24](https://helpdesk.bitrix24.com/open/11315016/)

## Current API Version

A deal is one of the CRM object types, so it is managed by the [universal methods](../universal/index.md) `crm.item.*` with `entityTypeId = 2`. The `crm.deal.*` methods remain only to support existing integrations.

#|
|| **If You Need To** | **Open the Method** ||
|| Create a deal | [crm.item.add](../universal/crm-item-add.md) ||
|| Update a deal | [crm.item.update](../universal/crm-item-update.md) ||
|| Retrieve a deal by its identifier | [crm.item.get](../universal/crm-item-get.md) ||
|| Retrieve a list of deals by filter | [crm.item.list](../universal/crm-item-list.md) ||
|| Delete a deal | [crm.item.delete](../universal/crm-item-delete.md) ||
|| Retrieve the description of deal fields | [crm.item.fields](../universal/crm-item-fields.md) ||
|| Manage the product items of a deal | [crm.item.productrow.*](../universal/product-rows/index.md) with `ownerType = D` ||
|| Configure the deal card | [crm.item.details.configuration.*](../universal/item-details-configuration/index.md) ||
|#

In the universal methods, field names are written in `camelCase`: `TITLE` becomes `title`, and `ASSIGNED_BY_ID` becomes `assignedById`. Some fields are named differently: the deal stage `STAGE_ID` arrives in the `stageId` field, the funnel `CATEGORY_ID` — in the `categoryId` field, and the multiple field `CONTACT_IDS` — in the `contactIds` field. The exact set of fields is returned by the method [crm.item.fields](../universal/crm-item-fields.md) with `entityTypeId = 2`.

## Getting Started

The following sequence is intended for new development, based on the universal methods.

1. Retrieve the description of deal fields using the method [crm.item.fields](../universal/crm-item-fields.md) with `entityTypeId = 2` — it returns both system and custom fields with their types
2. Find out the available funnels using the method [crm.category.list](../universal/category/crm-category-list.md) with the parameter `entityTypeId = 2`, and their stages using the method [crm.status.list](../status/crm-status-list.md) with the filter `ENTITY_ID = DEAL_STAGE` for the default funnel or `DEAL_STAGE_<funnel_id>` for the rest
3. Create a deal using the method [crm.item.add](../universal/crm-item-add.md): pass `entityTypeId = 2`, the title `title`, the funnel `categoryId`, the stage `stageId`, the amount `opportunity` with the currency `currencyId`, and the client — the company `companyId` or the contacts `contactIds`
4. Add product items with the group of methods [crm.item.productrow.*](../universal/product-rows/index.md) using `ownerType = D`
5. Move the deal through stages and between funnels using the method [crm.item.update](../universal/crm-item-update.md)
6. Subscribe to the [deal events](./events/index.md) to receive notifications about changes in your application

## Connection of Deals with Other CRM Objects

**Client.** A field in the deal card that consists of the associated company and contacts. All activities related to calls, e-mails, and chats with the contact or company will be saved in the active deal card. There can be only one company in the field, and it is accessed directly through the deal field `COMPANY_ID`. Multiple contacts can be specified, and interaction with them is conducted through a separate group of methods [crm.deal.contact.*](./contacts/index.md).

**Products.** The product items of a deal are created, modified, and deleted by the group of methods [crm.item.productrow.*](../universal/product-rows/index.md). The deal is specified in them by the pair `ownerType = D` and `ownerId` with the deal identifier.

**Payments.** The payment documents for a deal are created, modified, and deleted by the group of methods [crm.item.payment.*](../universal/payment/index.md).

{% note tip "User Documentation" %}

- [Connection between deals, contacts, and companies](https://helpdesk.bitrix24.com/open/2519229/)
- [How to add products to deals, leads, and estimates](https://helpdesk.bitrix24.com/open/13323830/)
- [How to accept payments from clients and work with receipts in Bitrix24](https://helpdesk.bitrix24.com/open/18265916/)

{% endnote %}

## Sales Funnels and Deal Stages

Sales funnels are managed by the group of methods [crm.category.*](../universal/category/index.md) with `entityTypeId = 2`.

Each funnel has its own set of stages, managed by the group of CRM reference methods — [crm.status.*](../status/index.md). The stages of different funnels are stored in different references: for the default funnel, `ENTITY_ID` equals `DEAL_STAGE`, and for the rest — `DEAL_STAGE_<funnel_id>`, for example `DEAL_STAGE_5`.

You can retrieve the history of a deal's movement through stages using the method [crm.stagehistory.list](../crm-stage-history-list.md).

{% note tip "User Documentation" %}

- [Sales funnels: how to divide work by departments in CRM](https://helpdesk.bitrix24.com/open/20739996/)

{% endnote %}

### How to Change a Deal's Funnel

The method [crm.deal.update](./crm-deal-update.md) can only change the stage of a deal within the current funnel. If you pass a `STAGE_ID` that does not belong to the current funnel, nothing will change.

To move a deal to a stage in another funnel, use the method [crm.item.update](../universal/crm-item-update.md) with the following parameters:

- `entityTypeId` — `2` for the deal
- `id` — the identifier of the deal you are moving
- `categoryId` — the identifier of the funnel to which you are moving the deal. This can be obtained using the method [crm.category.list](../universal/category/crm-category-list.md)
- `stageId` — the identifier of the stage in the new funnel. This can be obtained using the method [crm.status.list](../status/crm-status-list.md)

Moving a deal triggers the event [onCrmDealMoveToCategory](./events/on-crm-deal-move-to-category.md), not [onCrmDealUpdate](./events/on-crm-deal-update.md). A request example and the response breakdown are available on the page of the method [crm.item.update](../universal/crm-item-update.md).

## Deal Card

The main workspace in a deal is the General tab of its card. It consists of two parts:

* the left part, which contains fields with information. If the system fields are insufficient, you can create your own custom fields. They allow you to store information in various data formats: string, number, link, address, and others. To create, modify, retrieve, or delete custom deal fields, the group of methods [crm.deal.userfield.*](./user-defined-fields/index.md) is used.

* the right part, which contains the deal's timeline. In it, you can create, edit, filter, and delete CRM activities — the group of methods [crm.activity.*](../timeline/activities/index.md), and timeline records — the group of methods [crm.timeline.*](../timeline/index.md).

The parameters of the deal card can be managed depending on the funnel through the group of methods [crm.deal.details.configuration.*](./custom-form/index.md).

{% note tip "User Documentation" %}

- [CRM Card: capabilities and settings](https://helpdesk.bitrix24.com/open/22879716/)
- [System fields in CRM](https://helpdesk.bitrix24.com/open/18529390/)
- [Custom fields in CRM](https://helpdesk.bitrix24.com/open/22067852/)
- [Timeline in a CRM object](https://helpdesk.bitrix24.com/open/16767378/)

{% endnote %}

## Widgets

You can embed an application into the deal card and work with it without leaving the card.

There are two embedding scenarios:

* use special [embedding locations](../../widgets/crm/index.md), for example, create your own tab
* create a [custom field](../../../tutorials/crm/crm-widgets/widget-as-field-in-lead-page.md) into which the interface of your application is loaded

{% note tip "Typical Use-Cases and Scenarios" %}

- [Widget Embedding Mechanism](../../widgets/index.md)
- [Embed a Widget in the CRM Card](../../../tutorials/crm/crm-widgets/widget-as-detail-tab.md)

{% endnote %}

## Recurring Deals

Similar deals can be created automatically from a template with a defined period and number of repetitions. Templates are managed by the group of methods [crm.deal.recurring.*](./recurring-deals/index.md). The tool is not available on every Bitrix24 plan; the details are on the [recurring deals](./recurring-deals/index.md) page.

## Overview of Methods and Events {#all-methods}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: depending on the method — the deal methods check the access permissions for deals, while creating, modifying, and deleting custom fields is available only to a CRM administrator. Any user can subscribe to the events

### Main

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [crm.deal.add](./crm-deal-add.md) | Creates a new deal ||
    || [crm.deal.update](./crm-deal-update.md) | Modifies a deal ||
    || [crm.deal.get](./crm-deal-get.md) | Returns a deal by ID ||
    || [crm.deal.list](./crm-deal-list.md) | Returns a list of deals by filter ||
    || [crm.deal.delete](./crm-deal-delete.md) | Deletes a deal and all associated objects ||
    || [crm.deal.fields](./crm-deal-fields.md) | Returns the description of deal fields ||
    || [crm.deal.productrows.set](./crm-deal-productrows-set.md) | Creates or updates the product items of a deal ||
    || [crm.deal.productrows.get](./crm-deal-productrows-get.md) | Returns the product items of a deal ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [onCrmDealAdd](./events/on-crm-deal-add.md) | When a deal is created ||
    || [onCrmDealUpdate](./events/on-crm-deal-update.md) | When a deal is modified ||
    || [onCrmDealDelete](./events/on-crm-deal-delete.md) | When a deal is deleted ||
    || [onCrmDealMoveToCategory](./events/on-crm-deal-move-to-category.md) | When a deal is moved to another funnel ||
    |#

{% endlist %}

### Recurring Deals

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [crm.deal.recurring.add](./recurring-deals/crm-deal-recurring-add.md) | Creates a recurring deal template ||
    || [crm.deal.recurring.update](./recurring-deals/crm-deal-recurring-update.md) | Modifies the settings of the recurring deal template ||
    || [crm.deal.recurring.get](./recurring-deals/crm-deal-recurring-get.md) | Returns the settings of the recurring deal template by its identifier ||
    || [crm.deal.recurring.list](./recurring-deals/crm-deal-recurring-list.md) | Returns a list of recurring deal templates ||
    || [crm.deal.recurring.delete](./recurring-deals/crm-deal-recurring-delete.md) | Deletes a recurring deal template ||
    || [crm.deal.recurring.expose](./recurring-deals/crm-deal-recurring-expose.md) | Creates a deal from the template outside the schedule ||
    || [crm.deal.recurring.fields](./recurring-deals/crm-deal-recurring-fields.md) | Returns the description of the recurring deal template fields ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [onCrmDealRecurringAdd](./recurring-deals/events/on-crm-deal-recurring-add.md) | When a recurring deal template is created ||
    || [onCrmDealRecurringUpdate](./recurring-deals/events/on-crm-deal-recurring-update.md) | When a recurring deal template is modified ||
    || [onCrmDealRecurringDelete](./recurring-deals/events/on-crm-deal-recurring-delete.md) | When a recurring deal template is deleted ||
    || [onCrmDealRecurringExpose](./recurring-deals/events/on-crm-deal-recurring-expose.md) | When a deal is created from the template ||
    |#

{% endlist %}

### Custom Fields

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [crm.deal.userfield.add](./user-defined-fields/crm-deal-userfield-add.md) | Creates a new custom field for deals ||
    || [crm.deal.userfield.update](./user-defined-fields/crm-deal-userfield-update.md) | Modifies an existing custom field for deals ||
    || [crm.deal.userfield.get](./user-defined-fields/crm-deal-userfield-get.md) | Returns a custom field for deals by its identifier ||
    || [crm.deal.userfield.list](./user-defined-fields/crm-deal-userfield-list.md) | Returns a list of custom fields for deals ||
    || [crm.deal.userfield.delete](./user-defined-fields/crm-deal-userfield-delete.md) | Deletes a custom field for deals ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [onCrmDealUserFieldAdd](./user-defined-fields/events/on-crm-deal-user-field-add.md) | When a custom field is added ||
    || [onCrmDealUserFieldUpdate](./user-defined-fields/events/on-crm-deal-user-field-update.md) | When a custom field is modified ||
    || [onCrmDealUserFieldDelete](./user-defined-fields/events/on-crm-deal-user-field-delete.md) | When a custom field is deleted ||
    || [onCrmDealUserFieldSetEnumValues](./user-defined-fields/events/on-crm-deal-user-field-set-enum-values.md) | When the set of values for a custom list-type field is changed ||
    |#

{% endlist %}

### Deal Contacts

#|
|| **Method** | **Description** ||
|| [crm.deal.contact.add](./contacts/crm-deal-contact-add.md) | Links a single contact to a deal ||
|| [crm.deal.contact.delete](./contacts/crm-deal-contact-delete.md) | Removes a single contact from a deal ||
|| [crm.deal.contact.items.get](./contacts/crm-deal-contact-items-get.md) | Returns the set of contacts linked to a deal ||
|| [crm.deal.contact.items.set](./contacts/crm-deal-contact-items-set.md) | Replaces the set of deal contacts with the one you pass ||
|| [crm.deal.contact.items.delete](./contacts/crm-deal-contact-items-delete.md) | Removes all contacts from a deal ||
|| [crm.deal.contact.fields](./contacts/crm-deal-contact-fields.md) | Returns the description of the fields for the deal-contact link ||
|#

### Managing Deal Cards

#|
|| **Method** | **Description** ||
|| [crm.deal.details.configuration.get](./custom-form/crm-deal-details-configuration-get.md) | Returns the settings for the deal card ||
|| [crm.deal.details.configuration.set](./custom-form/crm-deal-details-configuration-set.md) | Sets the settings for the deal card ||
|| [crm.deal.details.configuration.reset](./custom-form/crm-deal-details-configuration-reset.md) | Resets the settings for the deal card ||
|| [crm.deal.details.configuration.forceCommonScopeForAll](./custom-form/crm-deal-details-configuration-force-common-scope-for-all.md) | Forcefully sets a common deal card for all users ||
|#