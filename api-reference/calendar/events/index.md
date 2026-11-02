# Overview of Events When Working with Calendar Sections and Resources

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Events allow applications to respond to changes almost in real-time: receiving notifications about the addition, modification, or deletion of a calendar section and resource.

Detailed information on working with events is described in the article [Concept and Benefits of Event Handling](../../events/index.md).

> Quick navigation: [all events](#all-events)
>
> User documentation: [Bitrix24 Calendar](https://helpdesk.bitrix24.com/section/47483/)

## How to Receive Events

You can subscribe to calendar events through:

- [outbound webhook](../../../local-integrations/local-webhooks.md)
- [application](../../../settings/app-installation/index.md) and the method [event.bind](../../events/event-bind.md)

An example of a handler code for the event is described in the article [How to Test Your Handler for Handling Bitrix24 Events](../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../_includes/events-index.md) %}

## Event Data Structure {#event-data}

All events in this section transmit the same set of fields.

#|
|| **Parameter**
`type` | **Description** ||
|| **event**
[`string`](../../data-types.md) | Symbolic code of the event: `ONCALENDARSECTIONADD`, `ONCALENDARSECTIONUPDATE`, or `ONCALENDARSECTIONDELETE` ||
|| **event_handler_id**
[`integer`](../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../data-types.md) | Object with the event data. Contains a single key, `id` ||
|| **data.id**
[`string`](../../data-types.md) | Identifier of the calendar section or resource affected by the action. The object type is not transmitted in the event data. The way to determine the type is described in the [How to Distinguish a Calendar Section from a Resource](#section-or-resource) section ||
|| **ts**
[`timestamp`](../../data-types.md) | Date and time of the event sent from the [event queue](../../events/index.md) ||
|| **auth**
[`object`](../../data-types.md) | Authorization parameters and information about the Bitrix24 account where the event occurred. The structure is described in the article [Events: Overview of Methods and Events](../../events/index.md#auth) ||
|#

### How to Distinguish a Calendar Section from a Resource {#section-or-resource}

The `OnCalendarSection*` events are triggered for both a calendar section and a resource. Only the section `id` is transmitted in `data`. There is no separate type attribute in the event: a resource is stored as a calendar section with the `resource` type.

To determine the object by its identifier, compare `data.id` with the list of resources from [calendar.resource.list](../resource/calendar-resource-list.md). A match with an `ID` in the response means that the event relates to a resource. The fields of a regular calendar section are returned by [calendar.section.get](../calendar-section-get.md): pass the calendar `type` and `ownerId` and find the entry with the required `ID` in the response; the `CAL_TYPE` field contains the section type. There is no separate lookup of a section by `ID` alone in the API.

This approach does not work for the `ONCALENDARSECTIONDELETE` event: it is sent after deletion, and the methods no longer return the record.

A shortened payload example for a resource with `id = 197`; the full example with the `auth` parameters is on the [OnCalendarSectionAdd](./on-calendar-section-add.md) page:

```json
{
    "event": "ONCALENDARSECTIONADD",
    "data": {
        "id": "197"
    }
}
```

## Overview of Events {#all-events}

> Scope: [`calendar`](../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered by** ||
|| [OnCalendarSectionAdd](./on-calendar-section-add.md) |
- When a calendar section is added manually or by the method [calendar.section.add](../calendar-section-add.md)
- When a resource is added manually or by the method [calendar.resource.add](../resource/calendar-resource-add.md) ||
|| [OnCalendarSectionUpdate](./on-calendar-section-update.md) |
- When a calendar section is modified manually or by the method [calendar.section.update](../calendar-section-update.md)
- When a resource is modified manually or by the method [calendar.resource.update](../resource/calendar-resource-update.md) ||
|| [OnCalendarSectionDelete](./on-calendar-section-delete.md) |
- When a calendar section is deleted manually or by the method [calendar.section.delete](../calendar-section-delete.md)
- When a resource is deleted manually or by the method [calendar.resource.delete](../resource/calendar-resource-delete.md) ||
|#
