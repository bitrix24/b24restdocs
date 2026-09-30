# Overview of Events When Working with Custom Deal Fields

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Events allow applications to receive notifications regarding changes to custom deal field configurations: adding, changing, or deleting a field, as well as modifying the set of values for a list-type field.

They are not triggered when the value of a custom field is changed within a specific deal. To track a field value change in a deal, subscribe to the [onCrmDealUpdate](../../events/on-crm-deal-update.md) deal update event. The current field value can be retrieved using the [crm.deal.get](../../crm-deal-get.md) method.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../../events/index.md). Methods for creating, changing, and deleting fields are listed in the [Custom Fields for Deals](../index.md) overview.

> Quick navigation: [All Events](#all-events)

## How to Receive Events

You can subscribe to events for custom deal fields through:

- [Outbound webhook](../../../../../local-integrations/local-webhooks.md)
- [Application](../../../../../settings/app-installation/index.md) and the [event.bind](../../../../events/event-bind.md) method

An example of a handler for the event is described in the article [How to Test Your Handler for Processing Events in Bitrix24](../../../../events/test-handler.md).

## What the Handler Receives

All four events pass the same set of keys in `data.FIELDS`: the field ID `ID`, the object symbolic code `ENTITY_ID` with the value `CRM_DEAL`, and the field code `FIELD_NAME`.

```json
{
    "event": "ONCRMDEALUSERFIELDSETENUMVALUES",
    "data": {
        "FIELDS": {
            "ID": "6947",
            "ENTITY_ID": "CRM_DEAL",
            "FIELD_NAME": "UF_CRM_1736930561"
        }
    }
}
```

The field type, its settings, and the list values are not included in the event. To retrieve them, call the [crm.deal.userfield.get](../crm-deal-userfield-get.md) method with the `ID` from the event. After the `onCrmDealUserFieldDelete` event, the method returns the `ERROR_NOT_FOUND` error because the field no longer exists in Bitrix24. The full request with the `event_handler_id`, `ts`, and `auth` parameters is shown on the page of each event.

The `onCrmDealUserFieldSetEnumValues` event arrives paired with another event:

- when a list-type field is added — together with `onCrmDealUserFieldAdd`, even if the `LIST` parameter is not passed
- when the list is saved manually or via the `crm.deal.userfield.update` method with the `LIST` parameter — together with `onCrmDealUserFieldUpdate`, even if the set of values has not changed

## Server Availability for Sending and Receiving Events

{% include notitle [Server availability](../../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: Any user

#|
|| **Event** | **Triggered** ||
|| [onCrmDealUserFieldAdd](./on-crm-deal-user-field-add.md) | When a custom field is added manually or via the method [crm.deal.userfield.add](../crm-deal-userfield-add.md) ||
|| [onCrmDealUserFieldUpdate](./on-crm-deal-user-field-update.md) | When a custom field is changed manually or via the method [crm.deal.userfield.update](../crm-deal-userfield-update.md) ||
|| [onCrmDealUserFieldDelete](./on-crm-deal-user-field-delete.md) | When a custom field is deleted manually or via the method [crm.deal.userfield.delete](../crm-deal-userfield-delete.md) ||
|| [onCrmDealUserFieldSetEnumValues](./on-crm-deal-user-field-set-enum-values.md) | When the set of values for a list-type custom field is saved: when such a field is added manually or via the method [crm.deal.userfield.add](../crm-deal-userfield-add.md), or when the list is changed manually or via the method [crm.deal.userfield.update](../crm-deal-userfield-update.md) with the `LIST` parameter ||
|#
