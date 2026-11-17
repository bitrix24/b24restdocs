# Overview of Events When Working with Custom Contact Fields

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Events allow applications to respond to changes in almost real time: receive notifications when [custom contact fields](../index.md) are created, updated, or deleted, and when list field values are set.

The events report changes in the field structure, not in the values stored in contact cards. To track field value changes in a contact, subscribe to the [onCrmContactUpdate](../../events/on-crm-contact-update.md) event.

Detailed information on working with events is described in the article [Concept and Benefits of Event Handling](../../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to events for custom contact fields through:

- [outbound webhook](../../../../../local-integrations/local-webhooks.md)
- [application](../../../../../settings/app-installation/index.md) and the method [event.bind](../../../../events/event-bind.md)

An example of a handler for the event is described in the article [How to Test Your Handler for Bitrix24 Event Processing](../../../../events/test-handler.md).

## What the Handler Receives

All four events pass the same set of keys in `data.FIELDS`: the field identifier `ID`, the CRM object type `ENTITY_ID` with the value `CRM_CONTACT`, and the field code `FIELD_NAME`.

```json
{
    "event": "ONCRMCONTACTUSERFIELDSETENUMVALUES",
    "data": {
        "FIELDS": {
            "ID": "553",
            "ENTITY_ID": "CRM_CONTACT",
            "FIELD_NAME": "UF_CRM_1724770216"
        }
    }
}
```

The event does not pass the field settings or list values. To retrieve them, call [crm.contact.userfield.get](../crm-contact-userfield-get.md) with the `ID` from the event. This requires permission to read contacts. After the deletion event, the field is no longer available.

Each event page provides the full request with the `event_handler_id`, `ts`, and `auth` parameters.

If [crm.contact.userfield.add](../crm-contact-userfield-add.md) creates a list field with items, `onCrmContactUserFieldAdd` is triggered first. `onCrmContactUserFieldSetEnumValues` is triggered after the list items are saved. If [crm.contact.userfield.update](../crm-contact-userfield-update.md) passes the `LIST` array for a list field, `onCrmContactUserFieldUpdate` is triggered first, followed by `onCrmContactUserFieldSetEnumValues` after the items are saved.

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user. Administrator permissions are required for offline events

#|
|| **Event** | **Triggered** ||
|| [onCrmContactUserFieldAdd](./on-crm-contact-user-field-add.md) | When a custom field is added manually or via the method [crm.contact.userfield.add](../crm-contact-userfield-add.md) ||
|| [onCrmContactUserFieldUpdate](./on-crm-contact-user-field-update.md) | When custom field settings are changed manually or via the method [crm.contact.userfield.update](../crm-contact-userfield-update.md). The method triggers the event on every successful call, even if the settings have not changed ||
|| [onCrmContactUserFieldDelete](./on-crm-contact-user-field-delete.md) | When a custom field is deleted manually or via the method [crm.contact.userfield.delete](../crm-contact-userfield-delete.md) ||
|| [onCrmContactUserFieldSetEnumValues](./on-crm-contact-user-field-set-enum-values.md) | When values are set for a list custom field manually or via the methods [crm.contact.userfield.add](../crm-contact-userfield-add.md) and [crm.contact.userfield.update](../crm-contact-userfield-update.md), including when the set of values has not changed ||
|#