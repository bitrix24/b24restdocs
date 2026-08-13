# Contacts in CRM: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A contact is a CRM object that stores data about clients—individuals. The contact card contains phone numbers, email addresses, and messenger identifiers in a special format that allows communication with the client directly through Bitrix24.

{% note warning "" %}

Development of the `crm.contact.*` and `crm.contact.details.configuration.*` methods has been discontinued. For new development, use the universal methods `crm.item.*` with `entityTypeId = 3`. The company linking methods `crm.contact.company.*` and the custom field methods `crm.contact.userfield.*` continue to work.

{% endnote %}

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [contacts in Bitrix24](https://helpdesk.bitrix24.com/open/8401859/)

## Current API Version

A contact is one of the CRM object types, so it is managed by the [universal methods](../universal/index.md) `crm.item.*` with `entityTypeId = 3`. The `crm.contact.*` methods continue to work — retain them only in existing integrations.

#|
|| **Method with Discontinued Development** | **Replacement** ||
|| `crm.contact.add` | [crm.item.add](../universal/crm-item-add.md) ||
|| `crm.contact.update` | [crm.item.update](../universal/crm-item-update.md) ||
|| `crm.contact.get` | [crm.item.get](../universal/crm-item-get.md) ||
|| `crm.contact.list` | [crm.item.list](../universal/crm-item-list.md) ||
|| `crm.contact.delete` | [crm.item.delete](../universal/crm-item-delete.md) ||
|| `crm.contact.fields` | [crm.item.fields](../universal/crm-item-fields.md) ||
|| `crm.contact.details.configuration.*` | [crm.item.details.configuration.*](../universal/item-details-configuration/index.md) ||
|#

The [crm.contact.company.*](./company/index.md) and [crm.contact.userfield.*](./userfield/index.md) method groups have no replacement — they are current.

Field names differ between the two method groups: `crm.contact.*` accepts and returns fields in `UPPER_CASE` format, for example `LAST_NAME`, while `crm.item.*` uses `camelCase` format, for example `lastName`. The exact set of fields is returned by the method [crm.item.fields](../universal/crm-item-fields.md) with `entityTypeId = 3`.

## How to Get Started

1. Retrieve the description of contact fields with the [crm.contact.fields](./crm-contact-fields.md) method — it returns system and custom fields with their types.
2. Create a contact with the [crm.contact.add](./crm-contact-add.md) method. Fill in `NAME` or `LAST_NAME`: if both fields are empty, the method returns an error. Pass phone numbers and e-mail in the multiple fields `PHONE` and `EMAIL` — these are arrays of objects with the `VALUE` and `VALUE_TYPE` keys.
3. Link the contact to companies with the [crm.contact.company.*](./company/index.md) methods if the client represents several companies.
4. Find the contacts you need with the [crm.contact.list](./crm-contact-list.md) method: it accepts `filter`, `order`, and `select`, and returns the result in pages of 50 records.
5. Communicate with the client in the contact card — with activities [crm.activity.*](../timeline/activities/index.md) and timeline records [crm.timeline.*](../timeline/index.md).
6. Subscribe to [contact events](./events/index.md) to receive notifications about changes in your application.

## Relationships with Other CRM Objects

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

The parameters of the contact card can be managed through the group of methods [crm.contact.details.configuration.*](./custom-form/index.md).

{% note tip "User Documentation" %}

- [CRM Card: Features and Settings](https://helpdesk.bitrix24.com/open/22879716/)
- [System Fields in CRM](https://helpdesk.bitrix24.com/open/18529390/)
- [Custom Fields in CRM](https://helpdesk.bitrix24.com/open/22067852/)
- [Timeline in CRM object](https://helpdesk.bitrix24.com/open/16767378/)

{% endnote %}

## Widgets

You can embed an application into the contact card and work with it without leaving the card.

There are two embedding scenarios:

* use special [embedding locations](../../widgets/crm/index.md), for example, create your own tab
* create a [custom field](../../../tutorials/crm/crm-widgets/widget-as-field-in-lead-page.md) that loads the interface of your application

{% note tip "Typical use-cases and scenarios" %}

- [Widget Embedding Mechanism](../../widgets/index.md)
- [Embed a widget in the CRM card](../../../tutorials/crm/crm-widgets/widget-as-detail-tab.md)

{% endnote %}

## Overview of Methods and Events {#all-methods}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: depending on the method — the contact methods and the company linking methods check the access permissions for contacts, while creating, modifying, and deleting custom fields is available only to a CRM administrator. Any user can subscribe to the events

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
    || [crm.contact.fields](./crm-contact-fields.md) | Returns the description of contact fields, including custom fields ||
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
|| [crm.contact.company.delete](./company/crm-contact-company-delete.md) | Removes a company from the specified contact ||
|| [crm.contact.company.items.get](./company/crm-contact-company-items-get.md) | Returns the set of companies associated with the specified contact ||
|| [crm.contact.company.items.set](./company/crm-contact-company-items-set.md) | Sets the set of companies associated with the specified contact ||
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
    || [crm.contact.userfield.get](./userfield/crm-contact-userfield-get.md) | Returns a custom field for contacts by Id ||
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
|| [crm.contact.details.configuration.get](./custom-form/crm-contact-details-configuration-get.md) | Returns the settings of the contact card ||
|| [crm.contact.details.configuration.set](./custom-form/crm-contact-details-configuration-set.md) | Sets the settings of the contact card ||
|| [crm.contact.details.configuration.reset](./custom-form/crm-contact-details-configuration-reset.md) | Resets the settings of the contact card ||
|| [crm.contact.details.configuration.forceCommonScopeForAll](./custom-form/crm-contact-details-configuration-force-common-scope-for-all.md) | Forcefully sets a common contact card for all users ||
|#