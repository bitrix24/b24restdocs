# Overview of Events When Working with Contacts

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Events allow applications to respond to changes in almost real-time: receiving notifications about the creation, updating, or deletion of [contacts](../index.md). Events for creating, updating, and deleting custom contact fields are grouped separately — see [Overview of Events When Working with Custom Contact Fields](../userfield/events/index.md).

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to contact events through:

- [outbound webhook](../../../../local-integrations/local-webhooks.md)
- [application](../../../../settings/app-installation/index.md) and the method [event.bind](../../../events/event-bind.md)

An example of a handler for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../events/test-handler.md).

## What Data the Events Deliver

All three events share the same request structure — only the code in `event` differs. `data.FIELDS.ID` contains the contact ID; the event carries no field values.

```json
{
    "event": "ONCRMCONTACTADD",
    "event_handler_id": "11",
    "data": {
        "FIELDS": {
            "ID": "89"
        }
    },
    "ts": "1724695689",
    "auth": {
        "domain": "some-domain.bitrix24.com",
        "client_endpoint": "https://some-domain.bitrix24.com/rest/",
        "member_id": "a223c6b3710f85df22e9377d6c4f7553",
        "application_token": "51856fefc120afa4b628cc82d3935cce"
    }
}
```

Bitrix24 sends the request with content-type `application/x-www-form-urlencoded`, and all values are transmitted as strings. The example is abridged: the full set of `auth` keys, including tokens, is described on the event pages in the [Parameter auth](./on-crm-contact-add.md#auth) section.

After a contact is created or updated, retrieve its data with the [crm.contact.get](../crm-contact-get.md) method or the [crm.item.get](../../universal/crm-item-get.md) method with `entityTypeId = 3`. After deletion, the contact is no longer available, so retain the data you need on your side in advance.

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered** ||
|| [onCrmContactAdd](./on-crm-contact-add.md) | When a contact is created manually, via the [crm.contact.add](../crm-contact-add.md) method, or via the [crm.item.add](../../universal/crm-item-add.md) method with `entityTypeId = 3` ||
|| [onCrmContactUpdate](./on-crm-contact-update.md) | When a contact is updated manually, via the [crm.contact.update](../crm-contact-update.md) method, or via the [crm.item.update](../../universal/crm-item-update.md) method with `entityTypeId = 3` ||
|| [onCrmContactDelete](./on-crm-contact-delete.md) | When a contact is deleted manually, via the [crm.contact.delete](../crm-contact-delete.md) method, or via the [crm.item.delete](../../universal/crm-item-delete.md) method with `entityTypeId = 3` ||
|#