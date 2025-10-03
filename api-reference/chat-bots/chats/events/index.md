# Overview of Events When Working with Chats

Events allow applications to respond to changes in near real-time: receiving notifications when a bot is added or removed from a chat.

Detailed information on working with events is described in the article [Concept and Benefits of Event Handling](../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to chat events through:

-  [outgoing webhook](../../../../local-integrations/local-webhooks.md)

-  [application](../../../../settings/app-installation/index.md) and the method [event.bind](../../../events/event-bind.md)

An example of a handler for the event is described in the article [How to Test Your Handler for Handling Bitrix24 Events](../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`imbot`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered by** ||
|| [ONIMBOTJOINCHAT](./on-imbot-join-chat.md) | When a bot is manually added to a chat or by the method [imbot.chat.add](../imbot-chat-add.md), [imopenlines.crm.chat.user.add](../../../imopenlines/openlines/chats/imopenlines-crm-chat-user-add.md), and [im.chat.user.add](../../../chats/chat-users/im-chat-user-add.md) ||
|| [ONIMBOTDELETE](./on-imbot-delete.md) | When a chat bot is removed by the method [imbot.unregister](../../imbot-unregister.md) ||
|#