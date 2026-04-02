# Chats: Overview of Methods

This section describes the methods for creating group chats on behalf of a bot and managing participants, the owner, and managers.

> Quick Navigation: [All Methods](#all-methods)

## What You Can Do with Chats

- Create a new group chat
- Retrieve chat data
- Modify chat properties
- Add and remove participants
- Transfer ownership rights
- Manage the list of managers

## Overview of Methods {#all-methods}

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the methods: Owner of the registered bot

#|
|| **Method** | **Description** ||
|| [imbot.v2.Chat.add](./chat-add.md) | Creates a group chat ||
|| [imbot.v2.Chat.get](./chat-get.md) | Returns information about the chat ||
|| [imbot.v2.Chat.update](./chat-update.md) | Updates chat properties ||
|| [imbot.v2.Chat.User.add](./chat-user-add.md) | Adds participants to the chat ||
|| [imbot.v2.Chat.User.delete](./chat-user-delete.md) | Removes a participant from the chat ||
|| [imbot.v2.Chat.leave](./chat-leave.md) | Leaves the chat ||
|| [imbot.v2.Chat.User.list](./chat-user-list.md) | Returns a list of chat participants ||
|| [imbot.v2.Chat.Manager.add](./chat-manager-add.md) | Adds chat managers ||
|| [imbot.v2.Chat.Manager.delete](./chat-manager-delete.md) | Removes chat managers ||
|| [imbot.v2.Chat.setOwner](./chat-set-owner.md) | Assigns a new owner to the chat ||
|#

## Continue Your Exploration

- [Chatbots 2.0: Overview of Methods](../../index.md)
- [Messages imbot.v2](../messages/index.md)