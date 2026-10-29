# Linking Deals to Contacts: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The group of methods `crm.deal.contact.*` links contacts to a [deal](../index.md) and removes that link. A deal can have several linked contacts, one of which is considered primary. The methods `crm.deal.contact.*` work with a single contact, and the methods `crm.deal.contact.items.*` work with the entire set of contacts at once.

The group is current. Development of the main `crm.deal.*` methods has been discontinued in favor of `crm.item.*`, but the contact linking methods continue to work and are not marked as deprecated.

> Quick navigation: [all methods](#all-methods)

## How the Link Is Structured in the API

The link is described by three fields, and their composition is returned by the method [crm.deal.contact.fields](./crm-deal-contact-fields.md).

#|
|| **Field** | **Type** | **Description** ||
|| `CONTACT_ID` | [`integer`](../../../data-types.md) | Identifier of the contact, a required field. You can retrieve it using the method [crm.item.list](../../universal/crm-item-list.md) with `entityTypeId = 3` ||
|| `IS_PRIMARY` | [`char`](../../../data-types.md) | Indicates the primary contact of the deal, `Y` or `N`. A deal has only one primary contact, and its identifier is retained in the deal field `CONTACT_ID` ||
|| `SORT` | [`integer`](../../../data-types.md) | Order of the contact in the deal detail form. If you do not pass it, Bitrix24 sets the value itself — the rule differs between [crm.deal.contact.add](./crm-deal-contact-add.md) and [crm.deal.contact.items.set](./crm-deal-contact-items-set.md) ||
|#

A deal has only one company. Its identifier is retained in the field `companyId` — in the `crm.deal.*` methods it is `COMPANY_ID` — and is changed using the method [crm.item.update](../../universal/crm-item-update.md).

## How to Choose a Method

#|
|| **If You Need To** | **Open the Method** ||
|| Add a single contact without affecting the others | [crm.deal.contact.add](./crm-deal-contact-add.md) ||
|| Remove a single contact without affecting the others | [crm.deal.contact.delete](./crm-deal-contact-delete.md) ||
|| Retrieve the deal contacts with their order and the primary flag | [crm.deal.contact.items.get](./crm-deal-contact-items-get.md). The methods [crm.deal.get](../crm-deal-get.md) and [crm.deal.list](../crm-deal-list.md) do not return the set of contacts ||
|| Retrieve only the identifiers of the deal contacts | [crm.item.get](../../universal/crm-item-get.md) with `entityTypeId = 2`, the `contactIds` field ||
|| Replace the entire set of contacts and set `SORT` and the primary contact | [crm.deal.contact.items.set](./crm-deal-contact-items-set.md). Contacts that are not in the list you pass are unlinked ||
|| Change `SORT` or the primary contact for contacts that are already linked | [crm.deal.contact.items.set](./crm-deal-contact-items-set.md) — read the set using the method [crm.deal.contact.items.get](./crm-deal-contact-items-get.md) and pass it in full with the new values ||
|| Replace the set of contacts together with other deal fields | [crm.item.update](../../universal/crm-item-update.md) with `entityTypeId = 2` and the `contactIds` field. `SORT` is determined by the position of the identifier in the array, and the first contact becomes the primary one ||
|| Unlink all contacts from the deal | [crm.deal.contact.items.delete](./crm-deal-contact-items-delete.md) ||
|| Retrieve the description of the link fields | [crm.deal.contact.fields](./crm-deal-contact-fields.md) ||
|#

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
> Who can execute the method: depending on the method — reading the set of contacts requires the "read" access permission for the deal, and changing it requires the "modify" access permission for the deal. The exact permissions are listed on the method pages

#|
|| **Method** | **Description** ||
|| [crm.deal.contact.add](./crm-deal-contact-add.md) | Links a single contact to a deal ||
|| [crm.deal.contact.delete](./crm-deal-contact-delete.md) | Removes a single contact from a deal ||
|| [crm.deal.contact.items.get](./crm-deal-contact-items-get.md) | Returns the set of contacts linked to a deal ||
|| [crm.deal.contact.items.set](./crm-deal-contact-items-set.md) | Replaces the set of deal contacts with the one you pass ||
|| [crm.deal.contact.items.delete](./crm-deal-contact-items-delete.md) | Removes all contacts from a deal ||
|| [crm.deal.contact.fields](./crm-deal-contact-fields.md) | Returns the description of the fields for the deal-contact link ||
|#