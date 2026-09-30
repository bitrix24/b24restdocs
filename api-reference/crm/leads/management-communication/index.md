# Linking Leads to Contacts: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

You can link several contacts to a [lead](../index.md) — for example, if two of the client's employees communicate with you about the same request. Bitrix24 considers one of them the lead's primary contact. An application can add and remove contacts one at a time, as well as read and replace the lead's entire set of contacts.

Development of the lead-contact link methods continues, unlike development of the lead methods [crm.lead.*](../index.md).

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [The "Client" field in the CRM detail form](https://helpdesk.bitrix24.com/open/17748916/)

## Benefits of Linking Leads and Contacts

The linked contacts appear in the lead detail form and are used by other tools:

- the lead detail form displays the data of the contacts: name, phone number, email, position
- the lead detail form lets you call or send an email without navigating to the contact detail form
- emails, calls, and open channel chats with the client appear in both the contact detail form and the lead detail form until the lead is closed
- CoPilot in CRM processes client calls from the lead detail form: it transcribes recordings, summarizes conversations, and fills in fields in the CRM detail form
- when [generating documents from a template](../../document-generator/index.md), symbolic codes insert the data of the linked contacts into the document

{% note tip "User Documentation" %}

[CoPilot in CRM](https://helpdesk.bitrix24.com/open/19268296/)

{% endnote %}

## How the Link Works in the API

The link is a separate record, not a field of the lead, and it has its own fields. For example, this is what the `result` field looks like in the response of the [crm.lead.contact.items.get](./crm-lead-contact-items-get.md) method for a lead with two contacts:

```json
[
    {
        "CONTACT_ID": 1010,
        "SORT": 10,
        "ROLE_ID": 0,
        "IS_PRIMARY": "Y"
    },
    {
        "CONTACT_ID": 1020,
        "SORT": 20,
        "ROLE_ID": 0,
        "IS_PRIMARY": "N"
    }
]
```

#|
|| **Field** | **What It Means** | **Example Value** ||
|| `CONTACT_ID` | Contact identifier. The only required binding field. Contact identifiers are returned by the [crm.item.list](../../universal/crm-item-list.md) method with `entityTypeId = 3` | `1010` ||
|| `SORT` | Sort index: the methods return the lead's contacts in this order. If you do not pass a value, Bitrix24 assigns one automatically — the rule differs between [crm.lead.contact.add](./crm-lead-contact-add.md) and [crm.lead.contact.items.set](./crm-lead-contact-items-set.md) | `10` ||
|| `IS_PRIMARY` | Whether this is the lead's primary contact. Possible values: `Y` — yes, `N` — no | `Y` ||
|| `ROLE_ID` | Reserved field. The link methods do not accept it on write, and new bindings receive `0` | `0` ||
|#

Bitrix24 selects the primary contact itself and writes it to the lead's `CONTACT_ID` field, so you do not need to update this field separately.

#|
|| **What Happened** | **Which Contact Becomes Primary** ||
|| The first contact is linked to the lead | This contact, even if `IS_PRIMARY = N` is passed ||
|| The [crm.lead.contact.add](./crm-lead-contact-add.md) method added a contact with `IS_PRIMARY = Y` | The added contact, instead of the previous one ||
|| The [crm.lead.contact.items.set](./crm-lead-contact-items-set.md) method replaced the set | The first contact with `IS_PRIMARY = Y`, and if there is none, the first contact in the list ||
|| The [crm.lead.contact.delete](./crm-lead-contact-delete.md) method removed the primary contact | The first of the remaining contacts in ascending `SORT` order ||
|| All contacts are unlinked from the lead | None: the `CONTACT_ID` field is cleared ||
|#

## How to Link a Lead to Contacts

1. Find the required contacts using the [crm.item.list](../../universal/crm-item-list.md) method with `entityTypeId = 3` and note their identifiers.
2. Link a single contact with the [crm.lead.contact.add](./crm-lead-contact-add.md) method, or pass the whole list with the [crm.lead.contact.items.set](./crm-lead-contact-items-set.md) method. The second method replaces the entire set: the contacts that are not in the list are unlinked from the lead.
3. Check the result with the [crm.lead.contact.items.get](./crm-lead-contact-items-get.md) method — it returns the contacts with their sort index and the primary contact flag.
4. Remove an unnecessary contact with the [crm.lead.contact.delete](./crm-lead-contact-delete.md) method, or unlink all contacts with the [crm.lead.contact.items.delete](./crm-lead-contact-items-delete.md) method.

## Connections and the Repeat Lead Indicator

A repeat lead is a request from a client who is already in the company's customer database. Repeat leads have hidden contact information fields: "Phone", "Email", "Address", "Details". A repeat lead can only be converted into a deal. If CRM is configured to work with repeat leads, Bitrix24 automatically creates a repeat lead when a known client makes a new request and links it to the client's detail form.

{% note warning "" %}

The lead-contact link methods change the set of contacts and the lead's `CONTACT_ID` field, but do not change the repeat lead indicator `IS_RETURN_CUSTOMER`.

{% endnote %}

Bitrix24 recalculates the `IS_RETURN_CUSTOMER` indicator when it saves the lead itself. Therefore, for the indicator to be recalculated, pass the contact through the lead methods:

#|
|| **Call** | **List of Linked Contacts** | **`IS_RETURN_CUSTOMER` Indicator** ||
|| [crm.lead.contact.add](./crm-lead-contact-add.md), [crm.lead.contact.items.set](./crm-lead-contact-items-set.md) | Changed | Not changed ||
|| [crm.lead.add](../crm-lead-add.md), [crm.lead.update](../crm-lead-update.md) with the `CONTACT_ID` field | Changed | Recalculated ||
|| [crm.item.update](../../universal/crm-item-update.md) with `entityTypeId: 1` and the `contactIds` field | Changed | Recalculated ||
|| [crm.lead.contact.delete](./crm-lead-contact-delete.md) | The contact is removed from the list | Not changed ||
|| [crm.lead.contact.items.delete](./crm-lead-contact-items-delete.md) | The list is cleared | Not changed ||
|#

The exception is a change to a lead that is at a successful stage or moves to it: the indicator is not recalculated for such a lead. `IS_RETURN_CUSTOMER` cannot be passed directly: the field is read-only.

{% note tip "User Documentation" %}

[Repeat Leads and Deals](https://helpdesk.bitrix24.com/open/24147842/)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: a user with the "Edit" access permission for the lead; for [crm.lead.contact.items.get](./crm-lead-contact-items-get.md), the "Read" access permission is sufficient. The [crm.lead.contact.add](./crm-lead-contact-add.md), [crm.lead.contact.delete](./crm-lead-contact-delete.md), and [crm.lead.contact.items.set](./crm-lead-contact-items-set.md) methods additionally check the "Read" access permission for contacts. The field description from [crm.lead.contact.fields](./crm-lead-contact-fields.md) is available to a user with read access to leads, deals, or other CRM objects, including those in digital workspaces

#|
|| **Method** | **Description** ||
|| [crm.lead.contact.add](./crm-lead-contact-add.md) | Adds a contact to the specified lead ||
|| [crm.lead.contact.delete](./crm-lead-contact-delete.md) | Removes a contact from the specified lead ||
|| [crm.lead.contact.items.get](./crm-lead-contact-items-get.md) | Returns the set of contacts linked to the specified lead ||
|| [crm.lead.contact.items.set](./crm-lead-contact-items-set.md) | Establishes the set of contacts linked to the specified lead ||
|| [crm.lead.contact.items.delete](./crm-lead-contact-items-delete.md) | Clears the set of contacts linked to the specified lead ||
|| [crm.lead.contact.fields](./crm-lead-contact-fields.md) | Returns the description of fields for the lead-contact link ||
|#