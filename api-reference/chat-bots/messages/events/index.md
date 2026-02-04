# Overview of Events When Working with Messages

Events allow applications to respond to changes in near real-time: receiving notifications about the sending, modification, or deletion of messages in a chat with chatbots.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to message events through the [application](../../../../settings/app-installation/index.md) and the [event.bind](../../../events/event-bind.md) method.

An example of a handler for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`imbot`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered** ||
|| [ONIMBOTMESSAGEADD](./on-imbot-message-add.md) | When a message is sent in a chat with a chatbot manually or via the [imbot.message.add](../imbot-message-add.md) method ||
|| [ONIMBOTMESSAGEUPDATE](./on-imbot-message-update.md) | When a message is edited in a chat with a chatbot manually or via the [imbot.message.update](../imbot-message-update.md) method ||
|| [ONIMBOTMESSAGEDELETE](./on-imbot-message-delete.md) | When a message is deleted in a chat with a chatbot manually or via the [imbot.message.delete](../imbot-message-delete.md) method ||
|#