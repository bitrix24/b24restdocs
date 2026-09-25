# Companies in CRM: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A company is a CRM object that stores client data for legal entities. The company card contains:

- phone numbers, email addresses, and messenger identifiers in a special format. These allow direct communication with the client from Bitrix24
- details for generating invoices, contracts, and any other types of printed documents based on templates

{% note warning "Method Development Has Been Discontinued" %}

Development of the [Main Company Methods](#all-methods) and the card configuration methods [crm.company.details.configuration.*](./custom-form/index.md) has been discontinued. For new development, use the universal methods [crm.item.*](../universal/index.md) — the replacement table is in the section [Current API Version](#actual-version).

The [crm.company.contact.*](./contacts/index.md) and [crm.company.userfield.*](./userfields/index.md) methods remain current.

{% endnote %}

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [companies in Bitrix24](https://helpdesk.bitrix24.com/open/10709996/)

## Current API Version {#actual-version}

The base company methods and the methods for its card settings have been replaced by the universal CRM methods. A universal method works with different types of CRM objects and receives the type in the `entityTypeId` parameter. For a company, `entityTypeId` equals `4`.

#|
|| **Method with Discontinued Development** | **Replacement** ||
|| `crm.company.add` | [crm.item.add](../universal/crm-item-add.md) ||
|| `crm.company.update` | [crm.item.update](../universal/crm-item-update.md) ||
|| `crm.company.get` | [crm.item.get](../universal/crm-item-get.md) ||
|| `crm.company.list` | [crm.item.list](../universal/crm-item-list.md) ||
|| `crm.company.delete` | [crm.item.delete](../universal/crm-item-delete.md) ||
|| `crm.company.fields` | [crm.item.fields](../universal/crm-item-fields.md) ||
|| `crm.company.details.configuration.get` | [crm.item.details.configuration.get](../universal/item-details-configuration/crm-item-details-configuration-get.md) ||
|| `crm.company.details.configuration.set` | [crm.item.details.configuration.set](../universal/item-details-configuration/crm-item-details-configuration-set.md) ||
|| `crm.company.details.configuration.reset` | [crm.item.details.configuration.reset](../universal/item-details-configuration/crm-item-details-configuration-reset.md) ||
|| `crm.company.details.configuration.forceCommonScopeForAll` | [crm.item.details.configuration.forceCommonScopeForAll](../universal/item-details-configuration/crm-item-details-configuration-forceCommonScopeForAll.md) ||
|#

The discontinued methods keep working — you do not have to rewrite existing integrations.

## How to Get Started

1. Retrieve the description of the company fields with the [crm.item.fields](../universal/crm-item-fields.md) method with `entityTypeId: 4`. It returns system and custom fields, their types, and whether they are required
2. Create a company with the [crm.item.add](../universal/crm-item-add.md) method or find the one you need with the [crm.item.list](../universal/crm-item-list.md) method. Pass `entityTypeId: 4` to both methods as well
3. Link the company to contacts with the [crm.company.contact.*](./contacts/index.md) group of methods, and to details with the [crm.requisite.*](../requisites/index.md) methods
4. Subscribe to [company events](./events/index.md) if your application has to react to changes

## Relationships with Other CRM Objects

**Deal, lead, SPA.** Any CRM object that has the standard field `Client` is linked to a company. The link is stored in the `COMPANY_ID` field, and in the universal methods, in the `companyId` field. Change it with the groups of methods for [deals](../deals/index.md), [leads](../leads/index.md), and [SPAs](../universal/index.md).

**Contact.** Multiple contacts can be associated with a single company. This connection is managed by the group of methods [crm.company.contact.*](./contacts/index.md). When an employee selects a company in the `Client` field of a deal or an SPA, Bitrix24 can fill in the field with the contacts related to the company.

**Details.** Details are a separate CRM object. Create and modify them with the methods of the [crm.requisite.*](../requisites/index.md) and [crm.address.*](../requisites/addresses/index.md) groups. In the company card, the details are displayed in the `Details` field.

{% note tip "User Documentation" %}

- [Connection between deals, contacts, and companies](https://helpdesk.bitrix24.com/open/2519229/)
- [Connections of details with CRM objects](../requisites/links/index.md)
- [Changes in working with addresses and details in CRM](https://helpdesk.bitrix24.com/open/11785262/)

{% endnote %}

## Company Card

The main workspace in a company is the "General" tab of its card. It consists of two parts:

- the left part, which contains fields with information. If the system fields are insufficient, you can create your own custom fields. These allow you to store information in various data formats: string, number, link, address, and others. The group of methods [crm.company.userfield.*](./userfields/index.md) is used to create, modify, retrieve, or delete custom fields for companies

- the right part, which contains the company timeline. CRM activities in the timeline are managed by the group of methods [crm.activity.*](../timeline/activities/index.md), and timeline records by the group of methods [crm.timeline.*](../timeline/index.md). Both sets of methods create, modify, filter, and delete their objects

The parameters of the company card can be managed with the group of methods [crm.item.details.configuration.*](../universal/item-details-configuration/index.md) with `entityTypeId: 4`.

{% note tip "User Documentation" %}

- [CRM Card: Features and Settings](https://helpdesk.bitrix24.com/open/22879716/)
- [System Fields in CRM](https://helpdesk.bitrix24.com/open/18529390/)
- [Custom Fields in CRM](https://helpdesk.bitrix24.com/open/22067852/)
- [Timeline in CRM object](https://helpdesk.bitrix24.com/open/16767378/)

{% endnote %}

## Widgets

You can embed an application into the company card. The employee then works with the application without leaving the card.

There are two embedding scenarios:

- use special [embedding locations](../../widgets/crm/index.md). For example, create your own tab
- create a custom field where the interface of your application will be loaded. An example for a lead is described in the tutorial [How to Embed a Widget into a Lead as a Custom Field](../../../tutorials/crm/crm-widgets/widget-as-field-in-lead-page.md)

{% note tip "Typical use-cases and scenarios" %}

- [Widget Embedding Mechanism](../../widgets/index.md)
- [Embed a Widget in the CRM Card](../../../tutorials/crm/crm-widgets/widget-as-detail-tab.md)

{% endnote %}

## Company Events

An application can react to changes in companies in almost real time. The events of the section are split into two groups:

- [company events](./events/index.md) — creation, update, and deletion of a company
- [company custom field events](./userfields/events/index.md) — creation, update, and deletion of a field, as well as a change in the set of values of a list field

You can subscribe to the events through an outbound webhook or through an application and the method [event.bind](../../events/event-bind.md).

## Response Format {#response}

The methods of this section return the result in the `result` field and the request execution time in the [`time`](../../data-types.md#time) field. What `result` contains depends on the method:

- [crm.company.add](./crm-company-add.md) — the identifier of the created company
- [crm.company.update](./crm-company-update.md) and [crm.company.delete](./crm-company-delete.md) — `true`
- [crm.company.get](./crm-company-get.md) — the company object. Field names are in uppercase, for example `TITLE`
- [crm.company.list](./crm-company-list.md) — an array of companies, no more than 50 per request. Next to `result`, the response contains `total` — the number of companies found. If there is a next page, the response also contains `next` — the position from which to request it

Universal methods respond differently: [crm.item.get](../universal/crm-item-get.md) and [crm.item.add](../universal/crm-item-add.md) return the object in `result.item`, and [crm.item.list](../universal/crm-item-list.md) returns an array in `result.items`. Field names in them are in camelCase by default, for example `title`.

If the call fails, the response contains the `error` and `error_description` fields instead of `result`. For many errors of the main company methods, the `error` code is empty, and only the text describes the cause, for example `Not found`. Therefore, check for the `error` key or the HTTP status of the response rather than the code value.

In a [batch](../../../settings/how-to-call-rest-api/batch.md) request, command errors are returned with status `200`. How to recognize them is described in the [Error Codes](../../../error-codes.md) article, which also lists common REST API errors. Errors of a specific method are described on its page.

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
    || [crm.company.delete](./crm-company-delete.md) | Deletes a company ||
    || [crm.company.fields](./crm-company-fields.md) | Returns the description of company fields ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [onCrmCompanyAdd](./events/on-crm-company-add.md) | When a company is created manually or via the method [crm.company.add](./crm-company-add.md) ||
    || [onCrmCompanyUpdate](./events/on-crm-company-update.md) | When a company is updated manually or via the method [crm.company.update](./crm-company-update.md) ||
    || [onCrmCompanyDelete](./events/on-crm-company-delete.md) | When a company is deleted manually or via the method [crm.company.delete](./crm-company-delete.md) ||
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
    || [onCrmCompanyUserFieldAdd](./userfields/events/on-crm-company-user-field-add.md) | When a custom field is added manually or via the method [crm.company.userfield.add](./userfields/crm-company-userfield-add.md) ||
    || [onCrmCompanyUserFieldUpdate](./userfields/events/on-crm-company-user-field-update.md) | When a custom field is modified manually or via the method [crm.company.userfield.update](./userfields/crm-company-userfield-update.md) ||
    || [onCrmCompanyUserFieldDelete](./userfields/events/on-crm-company-user-field-delete.md) | When a custom field is deleted manually or via the method [crm.company.userfield.delete](./userfields/crm-company-userfield-delete.md) ||
    || [onCrmCompanyUserFieldSetEnumValues](./userfields/events/on-crm-company-user-field-set-enum-values.md) | When the set of values for a custom field of list type is changed manually or via the methods [crm.company.userfield.add](./userfields/crm-company-userfield-add.md) and [crm.company.userfield.update](./userfields/crm-company-userfield-update.md) ||
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
