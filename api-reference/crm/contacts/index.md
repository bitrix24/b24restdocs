# Contacts in CRM: Overview of Methods

A contact is a CRM object that stores data about clients—individuals. The contact card contains phone numbers, email addresses, and messenger identifiers in a special format that allows communication with the client directly through Bitrix.

> Quick navigation: [all methods and events](#all-methods) 
> 
> User documentation: [contacts in Bitrix24](https://helpdesk.bitrix24.com/open/8401859/) 

## Contact Connection with Other CRM Objects

**Deal, lead, SPA.** Any CRM object that has the standard field `Client` is connected to contacts. The connection is managed through groups of methods for [deals](../deals/index.md), [leads](../leads/index.md), and [SPAs](../universal/index.md). 

**Company.** A single contact can be linked to multiple companies. This connection is managed using the group of methods [crm.contact.company.*](./company/index.md). When you select a company in the `Client` field in deals or SPAs, all associated contacts are automatically pulled into the field. 

**Requisites.** The requisites themselves are a separate object, and methods from the group [crm.requisite.*](../requisites/index.md) and [crm.address.*](../requisites/addresses/index.md) are used to create or modify them. They are displayed in the `Requisites` field of the contact card. 

{% note tip "User Documentation" %}

- [Connection between deals, contacts, and companies](https://helpdesk.bitrix24.com/open/2519229/)
- [Requisite connections with CRM objects](../requisites/links/index.md)
- [Changes in working with addresses and requisites in CRM](https://helpdesk.bitrix24.com/open/11785262/)

{% endnote %}

## Contact Card

The main workspace in a contact is the General tab of its card. It consists of two parts: 

* The left part contains fields with information. If the system fields are insufficient, you can create your own custom fields. They allow you to store information in various data formats: string, number, link, address, and others. The group of methods [crm.contact.userfield.*](./userfield/index.md) is used to create, modify, retrieve, or delete custom contact fields.

* The right part contains the contact's timeline. In it, you can create, edit, filter, and delete CRM activities using the group of methods [crm.activity.*](../timeline/activities/index.md), and timeline records using the group of methods [crm.timeline.*](../timeline/index.md).

You can manage the parameters of the contact card through the group of methods [crm.contact.details.configuration.*](./custom-form/index.md). 

{% note tip "User Documentation" %}

- [CRM Card: Features and Settings](https://helpdesk.bitrix24.com/open/22879716/)
- [System Fields in CRM](https://helpdesk.bitrix24.com/open/18529390/)
- [Custom Fields in CRM](https://helpdesk.bitrix24.com/open/22067852/)
- [Timeline in CRM Entity](https://helpdesk.bitrix24.com/open/16767378/)

{% endnote %}

## Widgets

You can embed an application into the contact card. Thanks to embedding, you can use the application without leaving the contact card.

There are two embedding scenarios:

* Use special [embedding spots](../../widgets/crm/index.md). For example, by creating your own tab.
* Create a [custom field](../../../tutorials/crm/crm-widgets/widget-as-field-in-lead-page.md) that will load the interface of your application.

{% note tip "Typical Use-Cases and Scenarios" %}

- [Widget Embedding Mechanism](../../widgets/index.md)
- [Embed a Widget in the CRM Card](../../../tutorials/crm/crm-widgets/widget-as-detail-tab.md)

{% endnote %}

## Overview of Methods and Events {#all-methods}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute methods: depending on the method

### Basic

{% list tabs %}

- Methods
  
    #| 
    || **Method** | **Description** ||
    || [crm.contact.add](./crm-contact-add.md) | Creates a new contact ||
    || [crm.contact.update](./crm-contact-update.md) | Updates an existing contact ||
    || [crm.contact.get](./crm-contact-get.md) | Returns a contact by ID ||
    || [crm.contact.list](./crm-contact-list.md) | Returns a list of contacts by filter ||
    || [crm.contact.delete](./crm-contact-delete.md) | Deletes a contact and all associated objects ||
    || [crm.contact.fields](./crm-contact-fields.md) | Returns the description of contact fields, including custom ones ||
    |#

- Events

    #| 
    || **Event** | **Triggered** ||
    || [onCrmContactAdd](./events/on-crm-contact-add.md) | When a contact is created ||
    || [onCrmContactUpdate](./events/on-crm-contact-update.md) | When a contact is updated ||
    || [onCrmContactDelete](./events/on-crm-contact-delete.md) | When a contact is deleted ||
    |#

{% endlist %}

### Companies

#| 
|| **Method** | **Description** ||
|| [crm.contact.company.add](./company/crm-contact-company-add.md) | Adds a company to the specified contact ||
|| [crm.contact.company.items.get](./company/crm-contact-company-items-get.md) | Retrieves the set of companies associated with the specified contact ||
|| [crm.contact.company.items.set](./company/crm-contact-company-items-set.md) | Sets the set of companies associated with the specified contact ||
|| [crm.contact.company.delete](./company/crm-contact-company-delete.md) | Removes a company from the specified contact ||
|| [crm.contact.company.items.delete](./company/crm-contact-company-items-delete.md) | Clears the set of companies associated with the specified contact ||
|| [crm.contact.company.fields](./company/crm-contact-company-fields.md) | Returns the description of fields for contact-company connection ||
|#

### Custom Fields

{% list tabs %}

- Methods

    #| 
    || **Method** | **Description** ||
    || [crm.contact.userfield.add](./userfield/crm-contact-userfield-add.md) | Creates a custom field for contacts ||
    || [crm.contact.userfield.update](./userfield/crm-contact-userfield-update.md) | Modifies an existing custom field for contacts ||
    || [crm.contact.userfield.get](./userfield/crm-contact-userfield-get.md) | Returns a custom field for contacts by ID ||
    || [crm.contact.userfield.list](./userfield/crm-contact-userfield-list.md) | Returns a list of custom fields for contacts ||
    || [crm.contact.userfield.delete](./userfield/crm-contact-userfield-delete.md) | Deletes a custom field for contacts ||
    |#

- Events

    #| 
    || **Event** | **Triggered** ||
    || [onCrmContactUserFieldAdd](./userfield/events/on-crm-contact-user-field-add.md) | When a custom field is added ||
    || [onCrmContactUserFieldUpdate](./userfield/events/on-crm-contact-user-field-update.md) | When a custom field is updated ||
    || [onCrmContactUserFieldDelete](./userfield/events/on-crm-contact-user-field-delete.md) | When a custom field is deleted ||
    || [onCrmContactUserFieldSetEnumValues](./userfield/events/on-crm-contact-user-field-set-enum-values.md) | When the set of values for a list-type custom field is changed ||
    |#

{% endlist %}

### Managing Contact Cards

#| 
|| **Method** | **Description** ||
|| [crm.contact.details.configuration.get](./custom-form/crm-contact-details-configuration-get.md) | Retrieves the settings of contact cards ||
|| [crm.contact.details.configuration.reset](./custom-form/crm-contact-details-configuration-reset.md) | Resets the settings of contact cards ||
|| [crm.contact.details.configuration.set](./custom-form/crm-contact-details-configuration-set.md) | Sets the settings of contact cards ||
|| [crm.contact.details.configuration.forceCommonScopeForAll](./custom-form/crm-contact-details-configuration-force-common-scope-for-all.md) | Allows forcing a common contact card for all users ||
|#