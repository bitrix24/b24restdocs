# Deals in CRM: Overview of Methods

A deal is one of the key objects in CRM, where you can:

* manage the process of selling a product or service, including tracking stages and accepting online payments
* engage in dialogue with the client: calls, e-mails, chats in open channels
* view the history of interactions: activities, timeline records

> Quick navigation: [all methods and events](#all-methods) 
> 
> User documentation: [deals in Bitrix24](https://helpdesk.bitrix24.com/open/11315016/)  

## Connection of Deals with Other CRM Objects

**Client.** A field in the deal card that consists of the associated company and contacts. All activities related to calls, e-mails, and chats with the contact or company will be saved in the active deal card. There can be only one company in the field, and it is accessed directly through the deal field `COMPANY_ID`. Multiple contacts can be specified, and interactions with them are managed through a separate group of methods [crm.deal.contact.*](./contacts/index.md).

**Products.** Adding, modifying, and deleting product items in deals is possible through the group of methods [crm.item.productrow.*](../universal/product-rows/index.md).

**Payments.** Adding, modifying, and deleting payment documents in deals is possible through the group of methods [crm.item.payment.*](../universal/payment/index.md).  

{% note tip "User Documentation" %}

- [Connection between deals, contacts, and companies](https://helpdesk.bitrix24.com/open/2519229/)
- [How to add products to deals, leads, and estimates](https://helpdesk.bitrix24.com/open/13323830/)
- [How to accept payment in the deal form](https://helpdesk.bitrix24.com/open/18265916/)

{% endnote %}

## Sales Funnels and Deal Stages

You can create various sales funnels for deals and manage them through the group of methods [crm.category.*](../universal/category/index.md) where `entityTypeId` of the deal = `2`.

Each funnel will have its own stages. These can be managed through the group of methods for CRM reference books — [crm.status.*](../status/index.md). The `ENTITY_ID` of deal statuses is unique for each direction — `DEAL_STAGE_xx`. 

You can obtain the history of a deal's movement through the stages of the current funnel using the method [crm.stagehistory.list](../crm-stage-history-list.md). 

{% note tip "User Documentation" %}

- [Sales funnels: how to divide work by departments in CRM](https://helpdesk.bitrix24.com/open/20739996/)

{% endnote %}

## Deal Card

The main workspace in a deal is the General tab of its card. It consists of two parts:

* the left part, which contains fields with information. If the system fields are insufficient, you can create your own custom fields. They allow you to store information in various data formats: string, number, link, address, and others. To create, modify, retrieve, or delete custom deal fields, the group of methods [crm.deal.userfield.*](./user-defined-fields/index.md) is used.

* the right part, which contains the deal's timeline. Here, you can create, edit, filter, and delete CRM activities — the group of methods [crm.activity.*](../timeline/activities/index.md), and timeline records — the group of methods [crm.timeline.*](../timeline/index.md).

You can manage the parameters of the deal card depending on the funnel through the group of methods [crm.deal.details.configuration.*](./custom-form/index.md).

{% note tip "User Documentation" %}

- [CRM item form features and settings](https://helpdesk.bitrix24.com/open/22879716/)
- [Standard fields in CRM](https://helpdesk.bitrix24.com/open/18529390/)
- [Custom fields in CRM](https://helpdesk.bitrix24.com/open/22067852/)
- [Timeline in CRM item form](https://helpdesk.bitrix24.com/open/16767378/)

{% endnote %}

## Widgets

You can embed an application into the deal card. By embedding, you can use the application without leaving the deal card.

There are two embedding scenarios:

* Use special [embedding locations](../../widgets/crm/index.md). For example, by creating your own tab.
* Create a [custom field](../../../tutorials/crm/crm-widgets/widget-as-field-in-lead-page.md), into which the interface of your application will be loaded.

{% note tip "Typical use-cases and scenarios" %}

- [Widget embedding mechanism](../../widgets/index.md)
- [Embed a widget in the CRM card](../../../tutorials/crm/crm-widgets/widget-as-detail-tab.md)

{% endnote %}

## Recurring Deals

Automatic creation of similar [recurring deals](https://helpdesk.bitrix24.com/open/17240254/) based on templates. To manage templates, the group of methods [crm.deal.recurring.*](./recurring-deals/index.md) is used.

## Overview of Methods and Events {#all-methods}

> Scope: [`crm`](../../scopes/permissions.md)
> 
> Who can execute the method: depending on the method

### Basic

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
    || [crm.deal.productrows.set](./crm-deal-productrows-set.md) | Adds products to a deal ||
    || [crm.deal.productrows.get](./crm-deal-get.md) | Returns products of a deal ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [onCrmDealAdd](./events/on-crm-deal-add.md) | When a deal is created ||
    || [onCrmDealUpdate](./events/on-crm-deal-update.md) | When a deal is modified ||
    || [onCrmDealDelete](./events/on-crm-deal-delete.md) | When a deal is deleted ||
    || [onCrmDealMoveToCategory](./events/on-crm-deal-move-to-category.md) | When the deal's funnel is changed ||
    |#

{% endlist %}
  
### Recurring Deals

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [crm.deal.recurring.add](./recurring-deals/crm-deal-recurring-add.md) | Creates a new recurring deal ||
    || [crm.deal.recurring.fields](./recurring-deals/crm-deal-recurring-fields.md) | Returns the list of fields for the recurring deal template ||
    || [crm.deal.recurring.expose](./recurring-deals/crm-deal-recurring-expose.md) | Creates a new deal from a template ||
    || [crm.deal.recurring.update](./recurring-deals/crm-deal-recurring-update.md) | Modifies existing settings for the recurring deal template ||
    || [crm.deal.recurring.get](./recurring-deals/crm-deal-recurring-get.md) | Retrieves the settings fields of the recurring deal template by ID ||
    || [crm.deal.recurring.list](./recurring-deals/crm-deal-recurring-list.md) | Retrieves the list of settings for recurring deal templates ||
    || [crm.deal.recurring.delete](./recurring-deals/crm-deal-recurring-delete.md) | Deletes existing settings for the recurring deal template ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [onCrmDealRecurringAdd](./recurring-deals/events/on-crm-deal-recurring-add.md) | When a new recurring deal is created ||
    || [onCrmDealRecurringUpdate](./recurring-deals/events/on-crm-deal-recurring-update.md) | When a recurring deal is modified ||
    || [onCrmDealRecurringDelete](./recurring-deals/events/on-crm-deal-recurring-delete.md) | When a recurring deal is deleted ||
    || [onCrmDealRecurringExpose](./recurring-deals/events/on-crm-deal-recurring-expose.md) | When a new deal is created from a recurring deal ||
    |#

{% endlist %}

### Custom Fields

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [crm.deal.userfield.add](./user-defined-fields/crm-deal-userfield-add.md) | Creates a new custom field for deals ||
    || [crm.deal.userfield.update](./user-defined-fields/crm-deal-userfield-update.md) | Modifies an existing custom field for deals ||
    || [crm.deal.userfield.get](./user-defined-fields/crm-deal-userfield-get.md) | Retrieves a custom field for deals by ID ||
    || [crm.deal.userfield.list](./user-defined-fields/crm-deal-userfield-list.md) | Retrieves the list of custom fields for deals ||
    || [crm.deal.userfield.delete](./user-defined-fields/crm-deal-userfield-delete.md) | Deletes a custom field for deals ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [onCrmDealUserFieldAdd](./user-defined-fields/events/on-crm-deal-user-field-add.md) | When a custom field is added ||
    || [onCrmDealUserFieldUpdate](./user-defined-fields/events/on-crm-deal-user-field-update.md) | When a custom field is modified ||
    || [onCrmDealUserFieldDelete](./user-defined-fields/events/on-crm-deal-user-field-delete.md) | When a custom field is deleted ||
    || [onCrmDealUserFieldSetEnumValues](./user-defined-fields/events/on-crm-deal-user-field-set-enum-values.md) | When the set of values for a custom field of list type is changed ||
    |#

{% endlist %}

### Deal Contacts

#|
|| **Method** | **Description** ||
|| [crm.deal.contact.add](./contacts/crm-deal-contact-add.md) | Adds a contact to a deal ||
|| [crm.deal.contact.items.set](./contacts/crm-deal-contact-items-set.md) | Adds multiple contacts to a deal ||
|| [crm.deal.contact.fields](./contacts/crm-deal-contact-fields.md) | Returns the fields for the deal-contact connection ||
|| [crm.deal.contact.items.get](./contacts/crm-deal-contact-items-get.md) | Retrieves the set of contacts associated with the deal ||
|| [crm.deal.contact.delete](./contacts/crm-deal-contact-delete.md) | Removes a contact from the specified deal ||
|| [crm.deal.contact.items.delete](./contacts/crm-deal-contact-items-delete.md) | Removes the set of contacts associated with the specified deal ||
|#

### Managing Deal Cards

#|
|| **Method** | **Description** ||
|| [crm.deal.details.configuration.get](./custom-form/crm-deal-details-configuration-get.md) | Retrieves the settings for deal cards ||
|| [crm.deal.details.configuration.reset](./custom-form/crm-deal-details-configuration-reset.md) | Resets the settings for deal cards ||
|| [crm.deal.details.configuration.set](./custom-form/crm-deal-details-configuration-set.md) | Allows setting the configurations for deal cards ||
|| [crm.deal.details.configuration.forceCommonScopeForAll](./custom-form/crm-deal-details-configuration-force-common-scope-for-all.md) | Forces a common deal card for all users ||
|#