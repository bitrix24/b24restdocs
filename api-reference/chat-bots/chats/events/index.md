# Overview of Events in Chat Operations

Events allow applications to respond to changes almost in real-time, such as receiving notifications when a bot is added or removed from a chat.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to chat events through:

- [outgoing webhook](../../../../local-integrations/local-webhooks.md)

- [application](../../../../settings/app-installation/index.md) and the method [event.bind](../../../events/event-bind.md)

An example of a handler for the event is described in the article [How to Test Your Handler for Processing Events in Bitrix24](../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`imbot`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

#| 
|| **Event** | **Triggered By** ||
|| [ONIMBOTJOINCHAT](./on-imbot-join-chat.md) | When a bot is manually added to a chat or via the methods [imbot.chat.add](../imbot-chat-add.md), [imopenlines.crm.chat.user.add](../../../imopenlines/openlines/chats/imopenlines-crm-chat-user-add.md), and [im.chat.user.add](../../../chats/chat-users/im-chat-user-add.md) ||
|#