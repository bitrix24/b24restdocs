# Overview of Events When Working with Messages

Events allow applications to respond to changes in almost real-time: receiving notifications about sending, modifying, or deleting messages in a dialogue with chatbots.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to message events through:

-  [outgoing webhook](../../../../local-integrations/local-webhooks.md)

-  [application](../../../../settings/app-installation/index.md) and the method [event.bind](../../../events/event-bind.md)

An example of a handler for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`imbot`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered By** ||
|| [ONIMBOTMESSAGEADD](./on-imbot-message-add.md) | When a message is sent in a dialogue with a chatbot manually or by the method [imbot.message.add](../imbot-message-add.md) ||
|| [ONIMBOTMESSAGEUPDATE](./on-imbot-message-update.md) | When a message is edited in a dialogue with a chatbot manually or by the method [imbot.message.update](../imbot-message-update.md) ||
|| [ONIMBOTMESSAGEDELETE](./on-imbot-message-delete.md) | When a message is deleted in a dialogue with a chatbot manually or by the method [imbot.message.delete](../imbot-message-delete.md) ||
|#