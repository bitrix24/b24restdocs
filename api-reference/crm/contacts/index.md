# Contacts in CRM: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A contact is a CRM object that stores data about clients—individuals. The contact card stores phone numbers, email addresses, and messenger identifiers that you can use to communicate with the client directly from Bitrix24.

{% note warning "" %}

Development of the `crm.contact.*`, `crm.contact.company.*`, `crm.contact.details.configuration.*`, and `crm.contact.userfield.*` methods has been discontinued. For new development, use the contact methods in [REST 3.0](../rest-v3/items/index.md) — see the replacement table in [Current API Version](#actual-version).

{% endnote %}

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [contacts in Bitrix24](https://helpdesk.bitrix24.com/open/8401859/)

## Current API Version {#actual-version}

Contact methods are available in [REST 3.0](../rest-v3/items/index.md) under the same names, for example `crm.contact.add`, but are called at `/rest/api/`. The `crm.contact.*` methods belong to the [previous API version](../previous-api.md) — retain them only in existing integrations. Replacements for all CRM methods are listed on the [Migrate to New CRM Methods](../migration.md) page.

#|
|| **Previous Version Method** | **REST 3.0 Equivalent** ||
|| `crm.contact.add` | [crm.contact.add](../rest-v3/items/crm-entity-add-rest-v3.md) ||
|| `crm.contact.update` | [crm.contact.update](../rest-v3/items/crm-entity-update-rest-v3.md) ||
|| `crm.contact.get` | [crm.contact.get](../rest-v3/items/crm-entity-get-rest-v3.md) ||
|| `crm.contact.list` | [crm.contact.list](../rest-v3/items/crm-entity-list-rest-v3.md) ||
|| `crm.contact.delete` | [crm.contact.delete](../rest-v3/items/crm-entity-delete-rest-v3.md) ||
|| `crm.contact.fields` | [crm.contact.field.list](../rest-v3/fields/crm-entity-field-list-rest-v3.md) ||
|| `crm.contact.company.*` | [crm.contact.get](../rest-v3/items/crm-entity-get-rest-v3.md) and [crm.contact.update](../rest-v3/items/crm-entity-update-rest-v3.md) with the `companiesId` field, [crm.contact.addfieldvalue](../rest-v3/items/crm-entity-add-field-value-rest-v3.md), [crm.contact.deletefieldvalue](../rest-v3/items/crm-entity-delete-field-value-rest-v3.md). Only [crm.contact.company.add](./company/crm-contact-company-add.md) can make a company primary when adding it without overwriting the company list ||
|| `crm.contact.details.configuration.*` | [crm.contact.detailpagelayout.*](../rest-v3/detail-page-layout/index.md). The `crm.contact.details.configuration.reset` method has no equivalent ||
|| `crm.contact.userfield.*` | [crm.contact.field.custom.*](../rest-v3/fields/index.md). Only [crm.contact.userfield.add](./userfield/crm-contact-userfield-add.md) can create `enumeration` (list) and `file` fields ||
|#

Field names differ: `crm.contact.*` accepts and returns fields in `UPPER_CASE` format, for example `LAST_NAME`, while REST 3.0 uses `camelCase`, for example `lastName`. Contact fields in REST 3.0 are described on the [Contact Fields](../rest-v3/items/fields-contact.md) page.

## How to Get Started {#start}

This sequence is for existing integrations that use previous-version methods. For new development, use the equivalents in [Current API Version](#actual-version).

1. Retrieve the description of contact fields with the [crm.contact.fields](./crm-contact-fields.md) method — it returns system and custom fields with their types.
2. Create a contact with the [crm.contact.add](./crm-contact-add.md) method. If `NAME` and `LAST_NAME` are not filled in, the contact is created with the last name `Contact #N`, where `N` is its identifier. Pass phone numbers and e-mail in the multiple fields `PHONE` and `EMAIL` — these are arrays of objects with the `VALUE` and `VALUE_TYPE` keys.
3. Link the contact to companies with the [crm.contact.company.*](./company/index.md) methods if the client represents several companies.
4. Find the contacts you need with the [crm.contact.list](./crm-contact-list.md) method: it accepts `filter`, `order`, and `select`, and returns the result in pages of 50 records.
5. Communicate with the client in the contact card — with activities [crm.activity.*](../timeline/activities/index.md) and timeline records [crm.timeline.*](../timeline/index.md).
6. Subscribe to [contact events](./events/index.md) to receive notifications about changes in your application.

Example contact data with a phone number and e-mail:

```json
{
    "NAME": "Klaus",
    "PHONE": [{"VALUE": "+4915123456789", "VALUE_TYPE": "WORK"}],
    "EMAIL": [{"VALUE": "klaus@example.com", "VALUE_TYPE": "WORK"}]
}
```

## Relationships with Other CRM Objects

**Deal, lead, SPA.** Any CRM object that has the standard field `Client` is connected to contacts. The connection is managed by the methods for [deals](../deals/index.md), [leads](../leads/index.md), and [SPAs in REST 3.0](../rest-v3/items/index.md): contact identifiers are passed in the contact linking fields of these objects.

**Company.** A single contact can be linked to multiple companies. This connection is managed using the group of methods [crm.contact.company.*](./company/index.md). When you select a company in the `Client` field in deals or SPAs, all associated contacts are automatically pulled into the field.

**Requisites.** Requisites are a separate object. Contact requisites are created and modified using the REST 3.0 methods [crm.contact.requisite.*](../rest-v3/requisites/index.md), and contact addresses using [crm.contact.address.*](../rest-v3/addresses/index.md). In the previous API version, these are the [crm.requisite.*](../requisites/index.md) and [crm.address.*](../requisites/addresses/index.md) methods, where a requisite is linked to a contact using the `ENTITY_TYPE_ID = 3` and `ENTITY_ID` fields. Requisites are displayed in the `Requisites` field of the contact card. For details, see [Requisite Links with CRM Objects](../requisites/links/index.md).

{% note tip "User Documentation" %}

- [Connection between deals, contacts, and companies](https://helpdesk.bitrix24.com/open/2519229/)
- [Changes in working with addresses and requisites in CRM](https://helpdesk.bitrix24.com/open/11785262/)

{% endnote %}

## Contact Card

The main workspace in a contact is the General tab of its card. It consists of two parts:

* The left part contains fields with information. If the system fields are insufficient, you can create your own custom fields. They allow you to store information in various data formats: string, number, link, address, and others. Custom contact fields are managed by the [crm.contact.userfield.*](./userfield/index.md) methods.

* The right part contains the contact's timeline with activities and records. The methods for working with them are listed in step 5 of [How to Get Started](#start).

The parameters of the contact card can be managed through the group of methods [crm.contact.details.configuration.*](./custom-form/index.md).

{% note tip "User Documentation" %}

- [CRM Card: Features and Settings](https://helpdesk.bitrix24.com/open/22879716/)
- [System Fields in CRM](https://helpdesk.bitrix24.com/open/18529390/)
- [Custom Fields in CRM](https://helpdesk.bitrix24.com/open/22067852/)
- [Timeline in CRM object](https://helpdesk.bitrix24.com/open/16767378/)

{% endnote %}

## Widgets

You can embed an application into the contact card and work with it without leaving the card.

There are two ways to embed an application:

* use special [embedding locations](../../widgets/crm/index.md), for example, create your own tab
* create a [custom field](../../../tutorials/crm/crm-widgets/widget-as-field-in-lead-page.md) that loads the interface of your application

{% note tip "Typical use-cases and scenarios" %}

- [Widget Embedding Mechanism](../../widgets/index.md)
- [Embed a widget in the CRM card](../../../tutorials/crm/crm-widgets/widget-as-detail-tab.md)

{% endnote %}

## Overview of Methods and Events {#all-methods}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: depending on the method. Contact methods check access permissions for contacts, and company linking methods check permissions for contacts and companies. Any user can retrieve and modify their own personal card settings. Shared settings can be changed by a user with the "Manage CRM item form layout" or "Edit CRM preferences" permission in CRM. Custom fields can be created, modified, and deleted by a user with the "Edit CRM preferences" permission in CRM. Any user can subscribe to events. Administrator permissions are required for offline events

### Basic

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [crm.contact.add](./crm-contact-add.md) | Creates a new contact ||
    || [crm.contact.update](./crm-contact-update.md) | Updates an existing contact ||
    || [crm.contact.get](./crm-contact-get.md) | Returns a contact by ID ||
    || [crm.contact.list](./crm-contact-list.md) | Returns a list of contacts by filter ||
    || [crm.contact.delete](./crm-contact-delete.md) | Deletes a contact ||
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
    || [onCrmContactUserFieldSetEnumValues](./userfield/events/on-crm-contact-user-field-set-enum-values.md) | When values are set for a list-type custom field ||
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