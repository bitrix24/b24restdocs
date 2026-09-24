# Overview of Events When Working with Open Channel Connectors

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Open Channel connector events notify the application about external channel messages, the start and completion of dialogues, channel status disconnection, and line deletion.

Detailed work with events is described in the article [Concept and Benefits of Event Handling](../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to connector events through the [application](../../../../settings/app-installation/index.md) and the [event.bind](../../../events/event-bind.md) method.

An example of a handler code for the event is described in the article [How to Test Your Handler for Handling Bitrix24 Events](../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../_includes/events-index.md) %}

## Event Data Format

The handler receives a POST request with the event code in `event`, event data in `data`, the sending time in `ts`, and authorization parameters in `auth`. The structure of `data` depends on the event:

- message events `OnImConnectorMessageAdd`, `OnImConnectorMessageUpdate`, and `OnImConnectorMessageDelete` pass `CONNECTOR`, `LINE`, and the `MESSAGES` array
- dialog events `OnImConnectorDialogStart` and `OnImConnectorDialogFinish` pass `CONNECTOR`, `LINE`, and the `DATA` array
- the `OnImConnectorStatusDelete` event passes lowercase `connector` and `line` keys
- the `OnImConnectorLineDelete` event passes the identifier of the deleted open channel as a number directly in `data`

Abbreviated request example for `OnImConnectorMessageAdd`:

```json
{
  "event": "ONIMCONNECTORMESSAGEADD",
  "event_handler_id": 555,
  "data": {
    "CONNECTOR": "myconnector",
    "LINE": 107,
    "MESSAGES": [
      {
        "im": { "chat_id": 1807, "message_id": 86497 },
        "message": { "user_id": 27, "text": "Hello!" },
        "chat": { "id": "channel-123" }
      }
    ]
  },
  "ts": 1773759161,
  "auth": { "domain": "example.bitrix24.com", "user_id": 27 }
}
```

The full set of nested objects and `auth` parameters is provided on each event page.

## Overview of Events {#all-events}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered** ||
|| [OnImConnectorMessageAdd](on-im-connector-message-add.md) | When a message is sent from Bitrix24 to an external channel ||
|| [OnImConnectorDialogStart](on-im-connector-dialog-start.md) | When a dialogue is created in an external channel ||
|| [OnImConnectorMessageUpdate](on-im-connector-message-update.md) | When a message is modified in an external channel ||
|| [OnImConnectorMessageDelete](on-im-connector-message-delete.md) | When a message is deleted in an external channel ||
|| [OnImConnectorDialogFinish](on-im-connector-dialog-finish.md) | When a dialogue is closed in an external channel ||
|| [OnImConnectorStatusDelete](on-im-connector-status-delete.md) | When a channel status is disconnected ||
|| [OnImConnectorLineDelete](on-im-connector-line-delete.md) | When an open line is deleted ||
|#
