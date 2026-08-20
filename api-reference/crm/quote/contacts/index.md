# Linking Estimates to Contacts: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The group of methods crm.quote.contact.* links contacts to an estimate and removes that link. An estimate can have several contacts, one of which is considered primary. The methods with items in their name work with the entire set at once, and the rest work with a single link.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [estimates in Bitrix24](https://helpdesk.bitrix24.com/open/17643444/)

## How the Link Is Structured in the API

The link is described by three fields, and their composition is returned by the method [crm.quote.contact.fields](./crm-quote-contact-fields.md).

#|
|| **Field** | **Type** | **Description** ||
|| `CONTACT_ID` | [`integer`](../../../data-types.md) | Identifier of the contact, a required field. You can retrieve it using the method [crm.contact.list](../../contacts/crm-contact-list.md) ||
|| `IS_PRIMARY` | [`char`](../../../data-types.md) | Indicates the primary contact of the estimate, `Y` or `N`. The first linked contact becomes the primary one, as does the contact for which `Y` is explicitly passed ||
|| `SORT` | [`integer`](../../../data-types.md) | Order of the contact in the estimate detail form ||
|#

The identifier of the primary contact is retained in the estimate field `CONTACT_ID` and is returned by the method [crm.quote.get](../crm-quote-get.md).

An estimate has a single company. Its identifier is retained in the field `COMPANY_ID` and is changed using the method [crm.quote.update](../crm-quote-update.md). A separate group of methods is needed only for contacts, because an estimate can have several of them.

## Getting Started

1. Retrieve the estimate identifier using the method [crm.quote.list](../crm-quote-list.md), and the contact identifiers using the method [crm.contact.list](../../contacts/crm-contact-list.md)
2. Read the current set of links using the method [crm.quote.contact.items.get](./crm-quote-contact-items-get.md) — it shows which contacts are already linked and which one of them is primary
3. Change the set using a suitable method — how to choose one is described below
4. Check the result by calling [crm.quote.contact.items.get](./crm-quote-contact-items-get.md) again

All methods of the group take the estimate identifier in the `id` parameter. The methods that change the set return `true` or `false` in `result`, and [crm.quote.contact.items.get](./crm-quote-contact-items-get.md) returns an array of links.

## How to Retrieve and Change the Set of Contacts

The multiple field `CONTACT_IDS` is available in the methods [crm.quote.add](../crm-quote-add.md) and [crm.quote.update](../crm-quote-update.md), but it is not returned by the methods [crm.quote.get](../crm-quote-get.md) and [crm.quote.list](../crm-quote-list.md) — the only way to read the contacts of an existing estimate is the method [crm.quote.contact.items.get](./crm-quote-contact-items-get.md).

The methods change the set of contacts in different ways, and this determines which one to choose:

- [crm.quote.contact.add](./crm-quote-contact-add.md) adds a single contact to those already linked. If the contact is already linked to the estimate, the method returns `false` and changes nothing
- [crm.quote.contact.items.set](./crm-quote-contact-items-set.md) replaces the entire set: contacts that are not in the list you pass are unlinked from the estimate
- [crm.quote.contact.delete](./crm-quote-contact-delete.md) removes a single contact from the estimate, and [crm.quote.contact.items.delete](./crm-quote-contact-items-delete.md) removes all of them at once

The link can also be changed using the universal method [crm.item.update](../../universal/crm-item-update.md) with `entityTypeId = 7` — there the field is named `contactIds`. It is suitable only for replacing the entire set: `SORT` is recalculated from the position of the contact in the array, and the first contact of the list becomes the primary one. To set the order and the primary contact explicitly, or to change a single link, use the methods of this group.

## Benefits of Linking Estimates to Contacts

- The estimate detail form displays the linked contacts in the client block
- The printed form of the estimate pulls the buyer details from the linked contact or company. To explicitly specify a pair of buyer and seller details, use the method [crm.requisite.link.register](../../requisites/links/crm-requisite-link-register.md), passing `ENTITY_TYPE_ID = 7` and the estimate identifier in `ENTITY_ID`
- [Generating documents from a template](../../document-generator/index.md) inserts the data of the linked contacts into the document by symbolic codes

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method — reading the links requires the "read" access permission for estimates, and changing them requires the "modify" permission

#|
|| **Method** | **Description** ||
|| [crm.quote.contact.add](./crm-quote-contact-add.md) | Links a single contact to an estimate ||
|| [crm.quote.contact.delete](./crm-quote-contact-delete.md) | Removes a single contact from an estimate ||
|| [crm.quote.contact.items.get](./crm-quote-contact-items-get.md) | Returns the set of contacts linked to an estimate ||
|| [crm.quote.contact.items.set](./crm-quote-contact-items-set.md) | Replaces the set of estimate contacts with the one you pass ||
|| [crm.quote.contact.items.delete](./crm-quote-contact-items-delete.md) | Removes all contacts from an estimate ||
|| [crm.quote.contact.fields](./crm-quote-contact-fields.md) | Returns the description of the fields for the estimate-contact link ||
|#
