# Overview of Events When Working with Open Channels

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Open channel events notify an application about two things: a session with a customer has started or closed, and a message in a chat has been added, edited, or deleted. These events are most commonly used by connector applications, which connect their own communication channel to open channels. For example, when a new message event occurs, the connector delivers the operator's reply to the customer in an external messenger.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

Every subscribed application receives events from all open channel connectors. Identify your own connector by its code — `connector_id` or `CONNECTOR`.

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to open channel events through the [application](../../../../settings/app-installation/index.md) and the [event.bind](../../../events/event-bind.md) method.

An example of a handler for the event is described in the article [How to Test Your Handler for Processing Events in Bitrix24](../../../events/test-handler.md).

## What the Handler Receives

Data arrives in the `data` field, and its content depends on the event.

**Session start and close.** `data.DATA` contains the connector, the chat, and the open channel:

```json
{
    "event": "ONSESSIONSTART",
    "data": {
        "DATA": {
            "connector": {
                "connector_id": "my_connector",
                "line_id": "1",
                "chat_id": "17",
                "user_id": "10"
            },
            "chat": {
                "chat_id": "17"
            },
            "line": {
                "id": "1",
                "name": "Open channel"
            }
        }
    }
}
```

- `connector_id` — connector code
- `line_id` and `line.id` — open channel ID, `line.name` — its name
- `connector.chat_id` and `chat.chat_id` — chat ID in Bitrix24
- `user_id` — customer ID in Bitrix24

**Customer messages.** When a customer writes, edits, or deletes a message through a connector, `data` contains only `DATA` with the `connector`, `chat`, and `message` objects. The content is the same for all three events, and `message.id` is the message ID in the external system.

**Operator and system messages.** Bitrix24 passes such messages to the connector so that it delivers them to the customer. Besides `DATA`, `data` contains `CONNECTOR` — the connector code, and `LINE` — the channel ID:

#|
|| **Event** | **What `DATA` contains** ||
|| [OnOpenLineMessageAdd](./on-open-line-message-add.md) | The `connector` and `message` objects: text, author, files, and the message ID in Bitrix24 ||
|| [OnOpenLineMessageUpdate](./on-open-line-message-update.md) | The `im` object contains the chat and message in Bitrix24, `message` contains the new text and an array of IDs in the external system, and `chat` contains the chat in the external system ||
|| [OnOpenLineMessageDelete](./on-open-line-message-delete.md) | An array of `im`, `message`, and `chat` objects. Here `message.id` is a single ID, not an array ||
|#

In message events, `connector.chat_id` and `chat.id` are the chat ID in the external system, not in Bitrix24.

{% note warning "" %}

Events about editing and deleting an operator message arrive only if the connector has confirmed its delivery with the [imconnector.send.status.delivery](../../imconnector/imconnector-send-status-delivery.md) method. Without confirmation, Bitrix24 does not send them, and no error occurs.

{% endnote %}

## How to Handle an Event

1. Check the connector code and ignore events from other connectors
2. Deliver the operator message from the [OnOpenLineMessageAdd](./on-open-line-message-add.md) event to the customer in the external system and confirm delivery with the [imconnector.send.status.delivery](../../imconnector/imconnector-send-status-delivery.md) method
3. For [OnOpenLineMessageUpdate](./on-open-line-message-update.md) and [OnOpenLineMessageDelete](./on-open-line-message-delete.md) events that contain the `CONNECTOR` key, find the operator message in the external system by `message.id`, then edit or delete it

## Server Availability for Sending and Receiving Events

{% include notitle [Server availability](../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered** ||
|| [OnSessionStart](./on-session-start.md) | When an Open Channel session starts, for example, because a customer writes to the channel or the [imopenlines.session.start](../sessions/imopenlines-session-start.md) method is called ||
|| [OnOpenLineMessageAdd](./on-open-line-message-add.md) | When a new message appears in an open channel chat — from a customer, an operator, or the system, including via the [imopenlines.crm.message.add](../messages/imopenlines-crm-message-add.md) method ||
|| [OnOpenLineMessageUpdate](./on-open-line-message-update.md) | When a message is edited: by a customer through a connector, or by an operator if the connector has confirmed delivery ||
|| [OnOpenLineMessageDelete](./on-open-line-message-delete.md) | When a message is deleted: by a customer through a connector, or by an operator if the connector has confirmed delivery ||
|| [OnSessionFinish](./on-session-finish.md) | When an Open Channel session closes. If the session waits for the customer's reply after the dialog is finished, the event arrives when the session closes completely ||
|#
