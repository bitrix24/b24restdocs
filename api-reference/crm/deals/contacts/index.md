# Linking Deals to Contacts: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The group of methods `crm.deal.contact.*` links contacts to a deal and removes that link. A deal can have several linked contacts, one of which is considered primary. The methods `crm.deal.contact.*` work with a single contact, and the methods `crm.deal.contact.items.*` work with the entire set of contacts at once.

> Quick navigation: [all methods](#all-methods)

## How the Link Is Structured in the API

The link is described by three fields, and their composition is returned by the method [crm.deal.contact.fields](./crm-deal-contact-fields.md).

#|
|| **Field** | **Type** | **Description** ||
|| `CONTACT_ID` | [`integer`](../../../data-types.md) | Identifier of the contact, a required field. You can retrieve it using the method [crm.contact.list](../../contacts/crm-contact-list.md) ||
|| `IS_PRIMARY` | [`char`](../../../data-types.md) | Indicates the primary contact of the deal, `Y` or `N`. The first linked contact becomes the primary one, as does the contact for which `Y` is explicitly passed ||
|| `SORT` | [`integer`](../../../data-types.md) | Order of the contact in the deal detail form ||
|#

A deal has only one company. Its identifier is retained in the deal field `COMPANY_ID` and is changed using the method [crm.deal.update](../crm-deal-update.md). A separate group of methods is needed only for contacts, because a deal can have several of them.

## How to Retrieve and Change the Set of Contacts

The multiple field `CONTACT_IDS` is available in the methods [crm.deal.add](../crm-deal-add.md) and [crm.deal.update](../crm-deal-update.md), but it is not returned by the methods [crm.deal.get](../crm-deal-get.md) and [crm.deal.list](../crm-deal-list.md). To read the contacts of an existing deal, use the method [crm.deal.contact.items.get](./crm-deal-contact-items-get.md).

The methods change the set of contacts in different ways, and this determines which one to choose:

- [crm.deal.contact.add](./crm-deal-contact-add.md) adds a single contact to those already linked. If the contact is already linked to the deal, the method returns `false` and changes nothing
- [crm.deal.contact.items.set](./crm-deal-contact-items-set.md) replaces the entire set: contacts that are not in the list you pass are unlinked from the deal
- [crm.deal.contact.delete](./crm-deal-contact-delete.md) removes a single contact from the deal, and [crm.deal.contact.items.delete](./crm-deal-contact-items-delete.md) removes all of them at once

The link can also be changed using the universal method [crm.item.update](../../universal/crm-item-update.md) with `entityTypeId = 2` — there the field is named `contactIds`.

## Benefits of Linking Deals to Contacts

- The deal detail form displays information about related contacts: name, phone number, e-mail, and position
- You can call a contact or send an e-mail directly from the deal detail form without navigating to the contact detail form
- E-mails, calls, and chats from open lines are retained in both the contact detail form and the deal detail form. Communications are not attached to closed deals
- CoPilot in CRM processes client calls from the deal detail form: it transcribes recordings, summarizes conversations, and fills in fields in the CRM detail form
- When [generating documents from a template](../../document-generator/index.md), symbolic codes automatically insert data from related contacts into the document

{% note tip "User Documentation" %}

- [CoPilot in CRM](https://helpdesk.bitrix24.com/open/19268296/)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method — changing the set of contacts requires the "modify" access permission for deals, and reading it requires the "read" permission

#|
|| **Method** | **Description** ||
|| [crm.deal.contact.add](./crm-deal-contact-add.md) | Links a single contact to a deal ||
|| [crm.deal.contact.delete](./crm-deal-contact-delete.md) | Removes a single contact from a deal ||
|| [crm.deal.contact.items.get](./crm-deal-contact-items-get.md) | Returns the set of contacts linked to a deal ||
|| [crm.deal.contact.items.set](./crm-deal-contact-items-set.md) | Replaces the set of deal contacts with the one you pass ||
|| [crm.deal.contact.items.delete](./crm-deal-contact-items-delete.md) | Removes all contacts from a deal ||
|| [crm.deal.contact.fields](./crm-deal-contact-fields.md) | Returns the description of the fields for the deal-contact link ||
|#