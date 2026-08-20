# Estimates in CRM: Overview of Methods and Events

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

An estimate is a CRM object that allows you to create printed documents and send them to clients before a deal. An estimate has a subject, a stage, an amount in a currency, a list of product items, and a client — a company and contacts.

{% note warning "" %}

Development of the `crm.quote.*` methods for working with estimates has been discontinued. For new development, use the universal methods `crm.item.*` with `entityTypeId = 7`. The custom field methods `crm.quote.userfield.*` continue to work.

{% endnote %}

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [estimates in Bitrix24](https://helpdesk.bitrix24.com/open/17643444/)

## Current API Version

An estimate is one of the CRM object types, so it is managed by the [universal methods](../universal/index.md) `crm.item.*` with `entityTypeId = 7`. The `crm.quote.*` methods remain only to support existing integrations.

#|
|| **If You Need To** | **Open the Method** ||
|| Create an estimate | [crm.item.add](../universal/crm-item-add.md) ||
|| Update an estimate | [crm.item.update](../universal/crm-item-update.md) ||
|| Retrieve an estimate by its identifier | [crm.item.get](../universal/crm-item-get.md) ||
|| Retrieve a list of estimates by filter | [crm.item.list](../universal/crm-item-list.md) ||
|| Delete an estimate | [crm.item.delete](../universal/crm-item-delete.md) ||
|| Retrieve the description of estimate fields | [crm.item.fields](../universal/crm-item-fields.md) ||
|| Manage the product items of an estimate | [crm.item.productrow.*](../universal/product-rows/index.md) with `ownerType = Q` ||
|| Replace the entire set of estimate contacts | [crm.item.update](../universal/crm-item-update.md) with the `contactIds` field ||
|#

In the universal methods, field names are written in `camelCase`: `TITLE` becomes `title`, and `ASSIGNED_BY_ID` becomes `assignedById`. The conversion rules for custom fields are described in the section [Universal CRM Methods](../universal/index.md).

Some fields are named differently in the universal methods: the estimate stage `STATUS_ID` arrives in the `stageId` field, and the multiple field `CONTACT_IDS` — in the `contactIds` field. The exact set of fields for an estimate is returned by the method [crm.item.fields](../universal/crm-item-fields.md) with `entityTypeId = 7`.

## Getting Started

1. Retrieve the description of estimate fields using the method [crm.quote.fields](./crm-quote-fields.md) — it returns both system and custom fields with their types
2. Find out the available stages using the method [crm.status.list](../status/crm-status-list.md) with the filter `ENTITY_ID = QUOTE_STATUS`, and the list of currencies using the method [crm.currency.list](../currency/crm-currency-list.md)
3. Create an estimate using the method [crm.quote.add](./crm-quote-add.md): pass the subject `TITLE`, the stage `STATUS_ID`, the client company `COMPANY_ID` or the contacts `CONTACT_IDS`, and your own company `MYCOMPANY_ID`
4. Add product items using the method [crm.quote.productrows.set](./crm-quote-product-rows-set.md); to check their composition, use the method [crm.quote.productrows.get](./crm-quote-product-rows-get.md)
5. Subscribe to [estimate events](./events/index.md) to receive notifications about changes in your application

## Connection of Estimates with Other CRM Entities

**Client.** The company and contacts the estimate is addressed to. An estimate has a single company, and its identifier is passed in the field `COMPANY_ID`. There can be several contacts, and their identifiers are passed as an array in the multiple field `CONTACT_IDS`. You can find the required identifiers using the methods [crm.company.list](../companies/crm-company-list.md) and [crm.contact.list](../contacts/crm-contact-list.md). To read and change the contacts of an estimate that already exists one by one, it is more convenient to use the group of methods [crm.quote.contact.*](./contacts/index.md): the `CONTACT_IDS` field is not returned by [crm.quote.get](./crm-quote-get.md) and [crm.quote.list](./crm-quote-list.md).

**Deal.** An estimate can be created based on a deal and vice versa. The deal identifier is stored in the estimate field `DEAL_ID` and can be passed to the methods [crm.quote.add](./crm-quote-add.md) and [crm.quote.update](./crm-quote-update.md).

**Lead.** If the estimate was issued for a lead, the lead identifier is stored in the field `LEAD_ID`. The field is filled in automatically when a lead is converted and can be modified using the same methods.

**Invoice.** An invoice is linked to an estimate using the universal method [crm.item.add](../universal/crm-item-add.md): pass `entityTypeId = 31` and the estimate identifier in the field `parentId7`.

**Products.** The product items of an estimate are created and updated by the method [crm.quote.productrows.set](./crm-quote-product-rows-set.md) and returned by [crm.quote.productrows.get](./crm-quote-product-rows-get.md). You can retrieve the product identifier for an item using the method [catalog.product.list](../../catalog/product/catalog-product-list.md).

**Details.** Buyer details are pulled into the estimate form from the associated contact or company. Seller details are taken from the company specified in the field `MYCOMPANY_ID`. To explicitly specify a pair of buyer and seller details, use the method [crm.requisite.link.register](../requisites/links/crm-requisite-link-register.md), passing `ENTITY_TYPE_ID = 7` and the estimate identifier in `ENTITY_ID`.

{% note tip "User Documentation" %}

- [How to add products to deals, leads, and estimates](https://helpdesk.bitrix24.com/open/14303190/)
- [How to use your company details](https://helpdesk.bitrix24.com/open/16059544/)

{% endnote %}

## Estimate Detail Form

The main workspace of an estimate is the *General* tab of its detail form. It consists of two parts:

- the left part contains fields with information. If the system fields are insufficient, add your own custom fields using the group of methods [crm.quote.userfield.*](./user-field/index.md). They store information in various data formats: string, number, link, address, and others. The name of such a field starts with the prefix `UF_CRM_`, and it is under this name that the field is passed to [crm.quote.add](./crm-quote-add.md) and returned by [crm.quote.get](./crm-quote-get.md)

- the right part contains the estimate timeline. CRM activities in it are created, updated, and deleted by the group of methods [crm.activity.*](../timeline/activities/index.md) — the estimate is specified there by the pair `OWNER_TYPE_ID = 7` and `OWNER_ID`. Timeline records are managed by the group of methods [crm.timeline.*](../timeline/index.md), where the estimate is specified by the pair `ENTITY_TYPE = quote` and `ENTITY_ID`

{% note tip "User Documentation" %}

- [CRM Detail Form: Features and Settings](https://helpdesk.bitrix24.com/open/22879716/)
- [System Fields in CRM](https://helpdesk.bitrix24.com/open/18529390/)
- [Custom Fields in CRM](https://helpdesk.bitrix24.com/open/22067852/)
- [Timeline in CRM object](https://helpdesk.bitrix24.com/open/16767378/)

{% endnote %}

## Widgets

You can embed an application into the estimate detail form and work with it without leaving the form. There are two embedding scenarios:

- occupy a special [embedding location](../../widgets/crm/index.md) — for example, create your own tab in the detail form
- create a [custom field](../../../tutorials/crm/crm-widgets/widget-as-field-in-lead-page.md) into which the content of your application is loaded

{% note tip "Typical use-cases and scenarios" %}

- [Widget Embedding Mechanism](../../widgets/index.md)
- [Embed a Widget in a CRM Detail Form](../../../tutorials/crm/crm-widgets/widget-as-detail-tab.md)

{% endnote %}

## Overview of Methods and Events {#all-methods}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: depending on the method — estimate methods check access permissions for estimates, while creating, updating, and deleting custom fields is available only to a CRM administrator

### Main Methods

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [crm.quote.add](./crm-quote-add.md) | Creates a new estimate ||
    || [crm.quote.update](./crm-quote-update.md) | Modifies an existing estimate ||
    || [crm.quote.get](./crm-quote-get.md) | Returns an estimate by its identifier ||
    || [crm.quote.list](./crm-quote-list.md) | Returns a list of estimates based on a filter ||
    || [crm.quote.delete](./crm-quote-delete.md) | Deletes an estimate ||
    || [crm.quote.fields](./crm-quote-fields.md) | Returns the description of estimate fields ||
    || [crm.quote.productrows.get](./crm-quote-product-rows-get.md) | Returns the product items of the estimate ||
    || [crm.quote.productrows.set](./crm-quote-product-rows-set.md) | Creates or updates the product items of the estimate ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [onCrmQuoteAdd](./events/on-crm-quote-add.md) | When an estimate is created manually or via the method [crm.quote.add](./crm-quote-add.md) ||
    || [onCrmQuoteUpdate](./events/on-crm-quote-update.md) | When an estimate is updated manually or via the method [crm.quote.update](./crm-quote-update.md) ||
    || [onCrmQuoteDelete](./events/on-crm-quote-delete.md) | When an estimate is deleted manually or via the method [crm.quote.delete](./crm-quote-delete.md) ||
    |#

{% endlist %}

### Contacts

#|
|| **Method** | **Description** ||
|| [crm.quote.contact.add](./contacts/crm-quote-contact-add.md) | Links a single contact to an estimate ||
|| [crm.quote.contact.delete](./contacts/crm-quote-contact-delete.md) | Removes a single contact from an estimate ||
|| [crm.quote.contact.items.get](./contacts/crm-quote-contact-items-get.md) | Returns the set of contacts linked to an estimate ||
|| [crm.quote.contact.items.set](./contacts/crm-quote-contact-items-set.md) | Replaces the set of estimate contacts with the one you pass ||
|| [crm.quote.contact.items.delete](./contacts/crm-quote-contact-items-delete.md) | Removes all contacts from an estimate ||
|| [crm.quote.contact.fields](./contacts/crm-quote-contact-fields.md) | Returns the description of the fields for the estimate-contact link ||
|#

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
    || [onCrmQuoteUserFieldAdd](./user-field/events/on-crm-quote-user-field-add.md) | When a custom field is added manually or via the method [crm.quote.userfield.add](./user-field/crm-quote-user-field-add.md) ||
    || [onCrmQuoteUserFieldUpdate](./user-field/events/on-crm-quote-user-field-update.md) | When a custom field is updated manually or via the method [crm.quote.userfield.update](./user-field/crm-quote-user-field-update.md) ||
    || [onCrmQuoteUserFieldDelete](./user-field/events/on-crm-quote-user-field-delete.md) | When a custom field is deleted manually or via the method [crm.quote.userfield.delete](./user-field/crm-quote-user-field-delete.md) ||
    || [onCrmQuoteUserFieldSetEnumValues](./user-field/events/on-crm-quote-user-field-set-enum-values.md) | When the set of values for a list-type custom field is changed manually or via the method [crm.quote.userfield.update](./user-field/crm-quote-user-field-update.md) ||
    |#

{% endlist %}
