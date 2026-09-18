# Overview of Events When Working with Company User Fields

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Events allow applications to respond to changes in almost real-time: receiving notifications about the creation, update, or deletion of company user fields.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../../events/index.md). The methods that manage the fields themselves are collected in the section [Custom Company Fields](../index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to company user field events through:

- [outbound webhook](../../../../../local-integrations/local-webhooks.md)
- [application](../../../../../settings/app-installation/index.md) and the method [event.bind](../../../../events/event-bind.md)

An example of a handler code for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../../events/test-handler.md).

## Data Sent to the Handler

The events report changes to the structure of user fields, not changes to values in company cards. For all four events, the handler receives the same data in `data.FIELDS`:

```json
{
    "event": "ONCRMCOMPANYUSERFIELDUPDATE",
    "data": {
        "FIELDS": {
            "ID": "6979",
            "ENTITY_ID": "CRM_COMPANY",
            "FIELD_NAME": "UF_CRM_1743165530"
        }
    }
}
```

- `ID` — user field identifier
- `ENTITY_ID` — identifier of the object to which the field belongs. For companies, it is always `CRM_COMPANY`
- `FIELD_NAME` — user field code

The example shows the relevant part of the request. The complete payload with the `event_handler_id`, `ts`, and `auth` parameters is provided on individual event pages, for example [onCrmCompanyUserFieldUpdate](./on-crm-company-user-field-update.md).

The field type, settings, and list values are not sent in the event. Distinguish events by the code in the `event` field, not by the contents of `data.FIELDS`.

## How to Process an Event

1. Use the `event` field to determine what change occurred
2. For add, update, and list value change events, pass the received `ID` to [crm.company.userfield.get](../crm-company-userfield-get.md) to retrieve the current field properties
3. For a delete event, use the data from `data.FIELDS` and information previously retained by the integration. After deletion, the field is no longer available through REST

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered** ||
|| [onCrmCompanyUserFieldAdd](./on-crm-company-user-field-add.md) | When a user field is added manually or via the method [crm.company.userfield.add](../crm-company-userfield-add.md) ||
|| [onCrmCompanyUserFieldUpdate](./on-crm-company-user-field-update.md) | When a user field is changed manually or via the method [crm.company.userfield.update](../crm-company-userfield-update.md) ||
|| [onCrmCompanyUserFieldDelete](./on-crm-company-user-field-delete.md) | When a user field is deleted manually or via the method [crm.company.userfield.delete](../crm-company-userfield-delete.md) ||
|| [onCrmCompanyUserFieldSetEnumValues](./on-crm-company-user-field-set-enum-values.md) | When the set of values for a list-type user field is changed manually or via the methods [crm.company.userfield.add](../crm-company-userfield-add.md) and [crm.company.userfield.update](../crm-company-userfield-update.md) ||
|#
