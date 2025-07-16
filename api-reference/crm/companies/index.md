# Companies in CRM: Overview of Methods

A company is a CRM object that stores client data for legal entities. The company card contains:
* phone numbers, email addresses, and messenger identifiers in a special format. These allow direct communication with the client from Bitrix.
* details for generating invoices, contracts, and any other types of printed documents based on templates.

> Quick navigation: [all methods and events](#all-methods) 
> 
> User documentation: [companies in Bitrix24](https://helpdesk.bitrix24.com/open/10709996/) 

## Company Connection with Other CRM Objects

**Deal, lead, SPA.** Any CRM object that has the standard field `Client` is linked to a company. The connection is managed through groups of methods for [deals](../deals/index.md), [leads](../leads/index.md), [SPAs](../universal/index.md), using the `COMPANY_ID` field.

**Contact.** Multiple contacts can be associated with a single company. This connection is managed using the group of methods [crm.company.contact.*](./contacts/index.md). When you select a company in the `Client` field of deals or SPAs, all related contacts are automatically pulled into the field.

**Details.** The details themselves are a separate object, and methods from the group [crm.requisite.*](../requisites/index.md) and [crm.address.*](../requisites/addresses/index.md) are used to create or modify them. They are displayed in the `Details` field of the company card.

{% note tip "User Documentation" %}

- [Connection between deals, contacts, and companies](https://helpdesk.bitrix24.com/open/2519229/)
- [Connections of details with CRM objects](../requisites/links/index.md)
- [Changes in working with addresses and details in CRM](https://helpdesk.bitrix24.com/open/11785262/)

{% endnote %}

## Company Card

The main workspace in a company is the General tab of its card. It consists of two parts:

* the left part, which contains fields with information. If the system fields are insufficient, you can create your own custom fields. These allow you to store information in various data formats: string, number, link, address, and others. The group of methods [crm.company.userfield.*](./userfields/index.md) is used to create, modify, retrieve, or delete custom fields for companies.

* the right part, which contains the contact timeline. In it, you can create, edit, filter, and delete CRM activities using the group of methods [crm.activity.*](../timeline/activities/index.md), and timeline records using the group of methods [crm.timeline.*](../timeline/index.md).

The parameters of the company card can be managed through the group of methods [crm.company.details.configuration.*](./custom-form/index.md).

{% note tip "User Documentation" %}

- [CRM Card: Features and Settings](https://helpdesk.bitrix24.com/open/22879716/)
- [System Fields in CRM](https://helpdesk.bitrix24.com/open/18529390/)
- [Custom Fields in CRM](https://helpdesk.bitrix24.com/open/22067852/)
- [Timeline in CRM Entity](https://helpdesk.bitrix24.com/open/16767378/)

{% endnote %}

## Widgets

You can embed an application into the company card. This allows you to use the application without leaving the company card.

There are two embedding scenarios:

* Use special [embedding locations](../../widgets/crm/index.md). For example, by creating your own tab.
* Create a [custom field](../../../tutorials/crm/crm-widgets/widget-as-field-in-lead-page.md) where the interface of your application will be loaded.

{% note tip "Typical use-cases and scenarios" %}

- [Widget Embedding Mechanism](../../widgets/index.md)
- [Embed a Widget in the CRM Card](../../../tutorials/crm/crm-widgets/widget-as-detail-tab.md)

{% endnote %}

## Overview of Methods and Events {#all-methods}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute methods: depending on the method

### Main

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [crm.company.add](./crm-company-add.md) | Creates a new company ||
    || [crm.company.update](./crm-company-update.md) | Updates an existing company ||
    || [crm.company.get](./crm-company-get.md) | Returns a company by ID ||
    || [crm.company.list](./crm-company-list.md) | Returns a list of companies by filter ||
    || [crm.company.delete](./crm-company-delete.md) | Deletes a company and all related objects ||
    || [crm.company.fields](./crm-company-fields.md) | Returns the description of company fields ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [onCrmCompanyAdd](./events/on-crm-company-add.md) | when a company is created ||
    || [onCrmCompanyUpdate](./events/on-crm-company-update.md) | when a company is updated ||
    || [onCrmCompanyDelete](./events/on-crm-company-delete.md) | when a company is deleted ||
    |#

{% endlist %}

### Custom Fields

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [crm.company.userfield.add](./userfields/crm-company-userfield-add.md) | Creates a new custom field for companies ||
    || [crm.company.userfield.update](./userfields/crm-company-userfield-update.md) | Updates an existing custom field for companies ||
    || [crm.company.userfield.get](./userfields/crm-company-userfield-get.md) | Returns a custom field for companies by ID ||
    || [crm.company.userfield.list](./userfields/crm-company-userfield-list.md) | Returns a list of custom fields for companies by filter ||
    || [crm.company.userfield.delete](./userfields/crm-company-userfield-delete.md) | Deletes a custom field for companies ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [onCrmCompanyUserFieldAdd](./userfields/events/on-crm-company-user-field-add.md) | when a custom field is added ||
    || [onCrmCompanyUserFieldUpdate](./userfields/events/on-crm-company-user-field-update.md) | when a custom field is modified ||
    || [onCrmCompanyUserFieldDelete](./userfields/events/on-crm-company-user-field-delete.md) | when a custom field is deleted ||
    || [onCrmCompanyUserFieldSetEnumValues](./userfields/events/on-crm-company-user-field-set-enum-values.md) | when the set of values for a custom field of list type is changed ||
    |#

{% endlist %}

### Contacts

#|
|| **Method** | **Description** ||
|| [crm.company.contact.add](./contacts/crm-company-contact-add.md) | Adds a contact to the specified company ||
|| [crm.company.contact.items.get](./contacts/crm-company-contact-items-get.md) | Returns a set of contacts associated with the specified company ||
|| [crm.company.contact.items.set](./contacts/crm-company-contact-items-set.md) | Sets the set of contacts associated with the specified company ||
|| [crm.company.contact.delete](./contacts/crm-company-contact-delete.md) | Deletes a contact from the specified company ||
|| [crm.company.contact.items.delete](./contacts/crm-company-contact-items-delete.md) | Clears the set of contacts associated with the specified company ||
|| [crm.company.contact.fields](./contacts/crm-company-contact-fields.md) | Returns the description of fields for the company-contact connection ||
|#

### Managing Company Cards

#|
|| **Method** | **Description** ||
|| [crm.company.details.configuration.get](./custom-form/crm-company-details-configuration-get.md) | Retrieves the settings for company cards ||
|| [crm.company.details.configuration.reset](./custom-form/crm-company-details-configuration-reset.md) | Resets the settings for company cards ||
|| [crm.company.details.configuration.set](./custom-form/crm-company-details-configuration-set.md) | Sets the settings for company cards ||
|| [crm.company.details.configuration.forceCommonScopeForAll](./custom-form/crm-company-details-configuration-force-common-scope-for-all.md) | Allows forcing a common company card for all users ||
|#