# Leads in CRM: Overview of Methods

A lead is the starting point of the Sales Funnel. Its card contains information about the client's interest in a product or service: filled CRM forms, e-mails, calls, and chats with the client.

The main goal of working with leads is to determine how promising they are and convert them into deals for further selling of the product or service.

> Quick navigation: [all methods and events](#all-methods)
> 
> User documentation: [leads in Bitrix24](https://helpdesk.bitrix24.com/open/19360846/) 

## Connection of Leads with Other CRM Objects

**Products.** Adding, modifying, and deleting product items in deals is possible through the group of methods [crm.item.productrow.*](../universal/product-rows/index.md).

**Deal.** The connection appears after converting a lead into a successful one.

**Client.** A field in the lead card consisting of the associated company and contacts. The field is available in the repeat lead form. If repeat leads are disabled, the linking field appears after creating a company or contact based on the lead. There is one company in the lead, and access to it is made directly through the `COMPANY_ID` field. Multiple contacts can be specified, and interaction with them is conducted through a separate group of methods [crm.lead.contact.*](./management-communication/index.md).  

{% note tip "User Documentation" %}

- [How to add products to deals, leads, and estimates](https://helpdesk.bitrix24.com/open/13323830/)
- [How to convert a lead](https://helpdesk.bitrix24.com/open/19359866/)
- [Repeat leads and deals](https://helpdesk.bitrix24.com/open/17720892/)
- [Deals in CRM: Overview of Methods](../deals/index.md)

{% endnote %}

## Lead Card

The main workspace in a lead is the General tab of its card. It consists of two parts:

* The left part contains fields with information. If the system fields are insufficient, you can create your own custom fields. They allow storing information in various data formats: string, number, link, address, and others. To create, modify, retrieve, or delete custom lead fields, the group of methods [crm.lead.userfield.*](./userfield/index.md) is used.

* The right part contains the deal timeline. In it, you can create, edit, filter, and delete CRM activities — the group of methods [crm.activity.*](../timeline/activities/index.md), and timeline records — the group of methods [crm.timeline.*](../timeline/index.md).

The parameters of the deal card can be managed depending on the funnel through the group of methods [crm.lead.details.configuration.*](./custom-form/index.md).

{% note tip "User Documentation" %}

- [CRM Card: Features and Settings](https://helpdesk.bitrix24.com/open/22879716/)
- [System Fields in CRM](https://helpdesk.bitrix24.com/open/18529390/)
- [Custom Fields in CRM](https://helpdesk.bitrix24.com/open/22067852/)
- [Timeline in CRM Entity](https://helpdesk.bitrix24.com/open/16767378/)

{% endnote %}

## Widgets

An application can be embedded into the lead card. Thanks to embedding, you can use the application without leaving the lead card.

There are two embedding scenarios:
* Use special [embedding locations](../../widgets/crm/index.md). For example, by creating your own tab.
* Create a [custom field](../../../tutorials/crm/crm-widgets/widget-as-field-in-lead-page.md), into which the content of your application will be loaded.

{% note tip "Typical Use-Cases and Scenarios" %}

- [Widget Embedding Mechanism](../../widgets/index.md)
- [Embed a Widget in the CRM Card](../../../tutorials/crm/crm-widgets/widget-as-detail-tab.md)

{% endnote %}

## Features

Leads in CRM may be absent — this happens if the simple mode of CRM operation is activated.

It is impossible to convert a lead using the REST API. You can only change the stage to successful without creating new objects.

{% note tip "User Documentation" %}

- [CRM Operating Modes](https://helpdesk.bitrix24.com/open/17627868/)
- [How to Convert a Lead](https://helpdesk.bitrix24.com/open/19359866/)

{% endnote %}

## Overview of Methods and Events {#all-methods}

> Scope: [`crm`](../../scopes/permissions.md)
> 
> Who can execute the method: depending on the method

### Main

{% list tabs %}

- Methods
  
    #|
    || **Method** | **Description** ||
    || [crm.lead.add](./crm-lead-add.md) | Creates a new lead ||
    || [crm.lead.update](./crm-lead-update.md) | Modifies a lead ||
    || [crm.lead.get](./crm-lead-get.md) | Returns a lead by ID ||
    || [crm.lead.list](./crm-lead-list.md) | Returns a list of leads by filter ||
    || [crm.lead.delete](./crm-lead-delete.md) | Deletes a lead and all associated objects ||
    || [crm.lead.fields](./crm-lead-fields.md) | Returns the description of lead fields ||
    || [crm.lead.productrows.set](./crm-lead-productrows-set.md) | Adds products to a lead ||
    || [crm.lead.productrows.get](./crm-lead-get.md) | Returns the products of a lead ||
    |#

- Events 

    #|
    || **Event** | **Triggered** ||
    || [onCrmLeadAdd](./events/on-crm-lead-add.md) | When a lead is added ||
    || [onCrmLeadUpdate](./events/on-crm-lead-update.md) | When a lead is modified ||
    || [onCrmLeadDelete](./events/on-crm-lead-delete.md) | When a lead is deleted ||
    |#

{% endlist %}

### Connection Between Leads and Contacts

#|
|| **Method** | **Description** ||
|| [crm.lead.contact.add](./management-communication/crm-lead-contact-add.md) | Adds a contact binding to the specified lead ||
|| [crm.lead.contact.delete](./management-communication/crm-lead-contact-delete.md) | Removes a contact binding from the specified lead ||
|| [crm.lead.contact.items.get](./management-communication/crm-lead-contact-items-get.md) | Retrieves a list of contacts associated with the lead ||
|| [crm.lead.contact.items.set](./management-communication/crm-lead-contact-items-set.md) | Attaches a list of contacts to the specified lead ||
|| [crm.lead.contact.items.delete](./management-communication/crm-lead-contact-items-delete.md) | Removes the list of contacts from the lead ||
|| [crm.lead.contact.fields](./management-communication/crm-lead-contact-fields.md) | Retrieves the description of fields for lead-contact connection, used by methods of the `crm.lead.contact.*` family ||
|#

### Custom Fields

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [crm.lead.userfield.add](./userfield/crm-lead-userfield-add.md) | Creates a new field ||
    || [crm.lead.userfield.update](./userfield/crm-lead-userfield-update.md) | Modifies a field ||
    || [crm.lead.userfield.get](./userfield/crm-lead-userfield-get.md) | Returns a field by code ||
    || [crm.lead.userfield.list](./userfield/crm-lead-userfield-list.md) | Returns a list of fields ||
    || [crm.lead.userfield.delete](./userfield/crm-lead-userfield-delete.md) | Deletes a field ||
    |#

- Events 

    #|
    || **Event** | **Triggered** ||
    || [onCrmLeadUserFieldAdd](./userfield/events/on-crm-lead-user-field-add.md) | When a custom field is added ||
    || [onCrmLeadUserFieldUpdate](./userfield/events/on-crm-lead-user-field-update.md) | When a custom field is modified ||
    || [onCrmLeadUserFieldDelete](./userfield/events/on-crm-lead-user-field-delete.md) | When a custom field is deleted ||
    || [onCrmLeadUserFieldSetEnumValues](./userfield/events/on-crm-lead-user-field-set-enum-values.md) | When the set of values for a custom field of list type is changed ||
    |#

{% endlist %}

### Managing Lead Cards 

#|
|| **Method** | **Description** ||
|| [crm.lead.details.configuration.get](./custom-form/crm-lead-details-configuration-get.md) | Retrieves the settings parameters of lead cards ||
|| [crm.lead.details.configuration.reset](./custom-form/crm-lead-details-configuration-reset.md) | Resets the settings of lead cards ||
|| [crm.lead.details.configuration.set](./custom-form/crm-lead-details-configuration-set.md) | Sets the settings of lead cards ||
|| [crm.lead.details.configuration.forceCommonScopeForAll](./custom-form/crm-lead-details-configuration-force-common-scope-for-all.md) | Forces a common lead card for all users ||
|#