# Linking Leads to Contacts: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Using the group of methods `crm.lead.contact.*`, you can establish, retrieve, or remove the connection between contacts and leads. A lead retains a list of linked contacts, and the primary contact from that list is additionally written to the lead field `CONTACT_ID`.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [The "Client" field in the CRM detail form](https://helpdesk.bitrix24.com/open/17748916/)

## How to Link a Lead to Contacts

1. Find the required contacts with the [crm.contact.list](../../contacts/crm-contact-list.md) method and note their identifiers.
2. Attach a single contact with the [crm.lead.contact.add](./crm-lead-contact-add.md) method, or the whole list at once with the [crm.lead.contact.items.set](./crm-lead-contact-items-set.md) method.
3. Check the result with the [crm.lead.contact.items.get](./crm-lead-contact-items-get.md) method — it returns the connections with their sort order and the primary connection indicator.
4. Detach an unnecessary contact with the [crm.lead.contact.delete](./crm-lead-contact-delete.md) method, or clear the entire list with the [crm.lead.contact.items.delete](./crm-lead-contact-items-delete.md) method.

## Connection Fields

The connection between a lead and a contact is a separate record with its own set of fields. Their composition is returned by the [crm.lead.contact.fields](./crm-lead-contact-fields.md) method.

#|
|| **Field** | **Description** ||
|| `CONTACT_ID` | Contact identifier. A required field of the connection ||
|| `SORT` | Position of the contact in the list. The lower the value, the higher the contact appears in the lead detail form ||
|| `IS_PRIMARY` | Primary connection indicator, `Y` or `N`. The primary contact is written to the lead field `CONTACT_ID` and is displayed first in the detail form ||
|#

A connection becomes primary if you pass `IS_PRIMARY: Y`, or if the lead does not yet have a primary contact.

## Connections and the Repeat Lead Indicator

A repeat lead is a request from a client who is already in the company's customer database. Repeat leads have hidden contact information fields: "Phone", "Email", "Address", "Details". A repeat lead can only be converted into a deal. When a known client makes a new request, a repeat lead will automatically be created, linked to the client’s detail form, if repeat sales mode is enabled in CRM.

{% note warning %}

The `crm.lead.contact.*` methods do not change the repeat lead indicator `IS_RETURN_CUSTOMER`. They fill in the lead field `CONTACT_ID`, but the lead itself remains a simple one.

{% endnote %}

The `IS_RETURN_CUSTOMER` indicator is recalculated only when the lead itself is saved. To make a lead a repeat one, pass the contact through the lead methods:

#|
|| **Call** | **List of Linked Contacts** | **`IS_RETURN_CUSTOMER` Indicator** ||
|| [crm.lead.contact.add](./crm-lead-contact-add.md), [crm.lead.contact.items.set](./crm-lead-contact-items-set.md) | Updated | Not changed ||
|| [crm.lead.add](../crm-lead-add.md), [crm.lead.update](../crm-lead-update.md) with the `CONTACT_ID` field | Updated | Set to `Y` ||
|| [crm.item.update](../../universal/crm-item-update.md) with `entityTypeId: 1` and the `contactIds` field | Updated | Set to `Y` ||
|| [crm.lead.contact.delete](./crm-lead-contact-delete.md), [crm.lead.contact.items.delete](./crm-lead-contact-items-delete.md) | Cleared | Not changed ||
|#

`IS_RETURN_CUSTOMER` cannot be passed directly: the system recalculates the value from the `CONTACT_ID` and `COMPANY_ID` fields.

{% note tip "User Documentation" %}

[Repeat Leads and Deals](https://helpdesk.bitrix24.com/open/24147842/)

{% endnote %}

## Benefits of Linking Leads and Contacts

1. The lead detail form displays information about linked contacts: name, phone number, email, position.

2. You can call or send an email directly from the lead detail form without navigating to the contact detail form.

3. Communication with the client: emails, calls, and chats from open channels will be stored in both the contact detail form and the lead detail form. Communications are not attached to closed leads.

4. CoPilot in CRM processes client calls from the lead detail form: it transcribes recordings, summarizes conversations, and fills in fields in the CRM detail form.

5. When [generating documents from a template](../../document-generator/index.md), you can use symbolic codes that automatically insert data from linked contacts into the document.

{% note tip "User Documentation" %}

[CoPilot in CRM](https://helpdesk.bitrix24.com/open/19268296/)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can perform the method: any user

#|
|| **Method** | **Description** ||
|| [crm.lead.contact.add](./crm-lead-contact-add.md) | Adds a contact link to the specified lead ||
|| [crm.lead.contact.delete](./crm-lead-contact-delete.md) | Removes a contact link from the specified lead ||
|| [crm.lead.contact.items.get](./crm-lead-contact-items-get.md) | Retrieves a list of contacts linked to the lead ||
|| [crm.lead.contact.items.set](./crm-lead-contact-items-set.md) | Attaches a list of contacts to the specified lead ||
|| [crm.lead.contact.items.delete](./crm-lead-contact-items-delete.md) | Removes a list of contacts from the lead ||
|| [crm.lead.contact.fields](./crm-lead-contact-fields.md) | Retrieves the description of fields for the lead-contact link used by the methods in the `crm.lead.contact.*` family ||
|#