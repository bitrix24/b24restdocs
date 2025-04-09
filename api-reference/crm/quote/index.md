# Estimates in CRM: Overview of Methods

An estimate is a CRM object that allows you to create printed documents and send them to the client before a deal.

> Quick navigation: [all methods and events](#all-methods)
> 
> User documentation: [estimates in Bitrix24](https://helpdesk.bitrix24.com/open/17643444/) 

## Linking Estimates with Other CRM Objects

**Deal.** An estimate can be created based on a deal and vice versa. The connection is established in the estimate field `DEAL_ID`.

**Products.** Adding, modifying, and deleting product items in estimates can be done through the group of methods [crm.item.productrow.*](../universal/product-rows/index.md). 

**Details.** The buyer's details are pulled into the estimate form from the associated contact or company. The seller's details are pulled from the field `MYCOMPANY_ID`.

**Client.** This field in the estimate card consists of the associated company and contacts. There is one company in the field, and it is accessed directly through the field `COMPANY_ID`. Multiple contacts can be specified, and they can be modified through an array of data in the multiple field `CONTACT_IDS`.

{% note tip "User Documentation" %}

- [How to add products to deals, leads, and estimates](https://helpdesk.bitrix24.com/open/14303190/)
- [How to use your company's details](https://helpdesk.bitrix24.com/open/16059544/)

{% endnote %}

## Estimate Card

The main workspace in an estimate is the General tab of its card. It consists of two parts: 

* The left part contains fields with information. If the system fields are insufficient, you can create your own custom fields. They allow you to store information in various data formats: string, number, link, address, and others. To create, modify, retrieve, or delete custom fields for estimates, the group of methods [crm.quote.userfield.*](./user-field/index.md) is used.

* The right part contains the estimate timeline. In it, you can create, edit, filter, and delete CRM activities — the group of methods [crm.activity.*](../timeline/activities/index.md), and timeline records — the group of methods [crm.timeline.*](../timeline/index.md).

{% note tip "User Documentation" %}

- [CRM Card: Features and Settings](https://helpdesk.bitrix24.com/open/22879716/)
- [System Fields in CRM](https://helpdesk.bitrix24.com/open/18529390/)
- [Custom Fields in CRM](https://helpdesk.bitrix24.com/open/22067852/)
- [Timeline in CRM Entity](https://helpdesk.bitrix24.com/open/16767378/)

{% endnote %}

## Widgets

You can embed an application into the estimate card. This allows you to use the application without leaving the estimate card. 

There are two embedding scenarios: 
* Use special [embedding locations](../../widgets/crm/index.md). For example, by creating your own tab.
* Create a [custom field](../../../tutorials/crm/crm-widgets/widget-as-field-in-lead-page.md), into which the content of your application will be loaded.

{% note tip "Typical use-cases and scenarios" %}

- [Widget Embedding Mechanism](../../widgets/index.md)
- [Embed a Widget in a CRM Card](../../../tutorials/crm/crm-widgets/widget-as-detail-tab.md)

{% endnote %}

## Overview of Methods and Events {#all-methods}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

### Main

{% list tabs %}

- Methods
  
    #|
    || **Method** | **Description** ||
    || [crm.quote.add](./crm-quote-add.md) | Creates a new estimate ||
    || [crm.quote.update](./crm-quote-update.md) | Modifies an existing estimate ||
    || [crm.quote.get](./crm-quote-get.md) | Returns an estimate by ID ||
    || [crm.quote.list](./crm-quote-list.md) | Returns a list of estimates by filter ||
    || [crm.quote.delete](./crm-quote-delete.md) | Deletes an estimate and all related objects ||
    || [crm.quote.fields](./crm-quote-fields.md) | Returns the description of estimate fields ||
    || [crm.quote.productrows.get](./crm-quote-product-rows-get.md) | Returns the product items of the estimate ||
    || [crm.quote.productrows.set](./crm-quote-product-rows-set.md) | Sets (creates or updates) the product items of the estimate ||
    |#

- Events 

    #|
    || **Event** | **Triggered** ||
    || [onCrmQuoteAdd](./events/on-crm-quote-add.md) | When an estimate is created ||
    || [onCrmQuoteUpdate](./events/on-crm-quote-update.md) | When an estimate is updated ||
    || [onCrmQuoteDelete](./events/on-crm-quote-delete.md) | When an estimate is deleted ||
    |#

{% endlist %}

### Custom Fields

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [crm.quote.userfield.add](./user-field/crm-quote-user-field-add.md) | Creates a new custom field for estimates ||
    || [crm.quote.userfield.update](./user-field/crm-quote-user-field-update.md) | Updates an existing custom field for estimates ||
    || [crm.quote.userfield.get](./user-field/crm-quote-user-field-get.md) | Returns a custom field for estimates by ID ||
    || [crm.quote.userfield.list](./user-field/crm-quote-user-field-list.md) | Returns a list of custom fields for estimates by filter ||
    || [crm.quote.userfield.delete](./user-field/crm-quote-user-field-delete.md) | Deletes a custom field for estimates ||
    |#

- Events 
  
    #|
    || **Event** | **Triggered** ||
    || [onCrmQuoteUserFieldAdd](./user-field/events/on-crm-quote-user-field-add.md) | When a custom field is added ||
    || [onCrmQuoteUserFieldUpdate](./user-field/events/on-crm-quote-user-field-update.md) | When a custom field is modified ||
    || [onCrmQuoteUserFieldDelete](./user-field/events/on-crm-quote-user-field-delete.md) | When a custom field is deleted ||
    || [onCrmQuoteUserFieldSetEnumValues](./user-field/events/on-crm-quote-user-field-set-enum-values.md) | When the set of values for a custom field of list type is changed ||
    |#

 {% endlist %}