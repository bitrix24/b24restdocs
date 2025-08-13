# Overview of Events When Working with Open Channels

Events allow applications to respond to changes almost in real-time: receiving notifications about the creation, updating, or deletion of messages in the open channel chat.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to open channel events through the [application](../../../app-installation/index.md) and the [event.bind](../../../events/event-bind.md) method.

An example of a handler code for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`imopenlines`](../../../scopes/permissions.md) 
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered By** ||
|| [OnSessionStart](./on-session-start.md) | When a chat is created manually or by the method [imopenlines.session.start](../sessions/imopenlines-session-start.md) ||
|| [OnOpenLineMessageAdd](./on-open-line-message-add.md) | When a message is added to the chat manually or by the method [imopenlines.crm.message.add](../messages/imopenlines-crm-message-add.md) ||
|| [OnOpenLineMessageUpdate](./on-open-line-message-update.md) | When a message in the chat is modified manually ||
|| [OnOpenLineMessageDelete](./on-open-line-message-delete.md) | When a message in the chat is deleted manually ||
|| [OnSessionFinish](./on-session-finish.md) | When the chat is closed manually ||
|#