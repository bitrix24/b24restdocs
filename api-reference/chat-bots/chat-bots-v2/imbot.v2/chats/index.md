# Chats: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The methods allow you to create group chats on behalf of a bot and manage participants, owners, and managers.

> Quick navigation: [all methods](#all-methods)

## How to Get Started {#how-to-start}

1. Create a group chat with the [imbot.v2.Chat.add](./chat-add.md) method. If no owner is specified, the bot itself becomes the owner.
2. Add participants with [imbot.v2.Chat.User.add](./chat-user-add.md) or pass them right away in `fields.userIds` when creating the chat.
3. If necessary, modify the chat properties with [imbot.v2.Chat.update](./chat-update.md), and the list of managers with [imbot.v2.Chat.Manager.add](./chat-manager-add.md).
4. Send messages to the chat using the methods of the [Messages](../messages/index.md) group.
5. When the bot is no longer needed in the chat, remove it with the [imbot.v2.Chat.leave](./chat-leave.md) method.

A full description of the Chat object fields is available in [Objects and Fields](../../entities.md#chat).

## Chat Identifiers {#identifiers}

The methods of this section use two different identifiers.

#|
|| **Identifier** | **Where It Is Used** | **Example** ||
|| `dialogId` | An input parameter of the methods for working with chats, messages, and files. For group chats — the string `chat{chatId}`, for private chats — a string with the user ID | `"chat142"`, `"5"` ||
|| `chatId` | The numeric chat ID in method responses and in event data. The `dialogId` of a group chat is built from it | `142` ||
|#

More details about the format — [Format of dialogId](../../index.md#dialog-id).

## Chat Roles {#roles}

#|
|| **Role** | **What It Grants** | **Methods to Manage It** ||
|| Owner | Full rights to the chat. Only the owner can assign and remove managers. The bot becomes the owner automatically if `fields.ownerId` is not specified when the chat is created | [imbot.v2.Chat.setOwner](./chat-set-owner.md) ||
|| Manager | Extended rights to manage the chat. Only a current participant of the chat can be made a manager — other IDs are ignored without an error | [imbot.v2.Chat.Manager.add](./chat-manager-add.md), [imbot.v2.Chat.Manager.delete](./chat-manager-delete.md) ||
|| Participant | Access to the chat messages | [imbot.v2.Chat.User.add](./chat-user-add.md), [imbot.v2.Chat.User.delete](./chat-user-delete.md), [imbot.v2.Chat.User.list](./chat-user-list.md) ||
|#

## Relationship with Other Objects {#relations}

**Bot.** All methods of this section are executed on behalf of a registered bot: every call passes `botId`, and for webhook authorization, `botToken` as well. You receive them when registering the bot — [Bots](../bots/index.md).

**Messages.** The chat is the recipient of the bot's messages. The chat identifier in the `dialogId` format is passed to the methods of the [Messages](../messages/index.md) and [Files](../files/index.md) groups.

**Events.** When the bot is added to a group chat, it receives the [ONIMBOTV2JOINCHAT](../events/events.md#onimbotv2joinchat) event. A typical reaction to it is to send a welcome message to the chat.

**Users.** Participants and managers are set as arrays of Bitrix24 user IDs. The description of the User object fields is available in [Objects and Fields](../../entities.md#user).

## Overview of Methods {#all-methods}

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the methods: owner of the registered bot

#| 
|| **Method** | **Description** ||
|| [imbot.v2.Chat.add](./chat-add.md) | Creates a group chat ||
|| [imbot.v2.Chat.get](./chat-get.md) | Returns information about the chat ||
|| [imbot.v2.Chat.update](./chat-update.md) | Updates chat properties ||
|| [imbot.v2.Chat.User.add](./chat-user-add.md) | Adds participants to the chat ||
|| [imbot.v2.Chat.User.delete](./chat-user-delete.md) | Removes a participant from the chat ||
|| [imbot.v2.Chat.leave](./chat-leave.md) | Leaves the chat ||
|| [imbot.v2.Chat.User.list](./chat-user-list.md) | Returns the list of chat participants ||
|| [imbot.v2.Chat.Manager.add](./chat-manager-add.md) | Adds chat managers ||
|| [imbot.v2.Chat.Manager.delete](./chat-manager-delete.md) | Removes chat managers ||
|| [imbot.v2.Chat.setOwner](./chat-set-owner.md) | Assigns a new owner to the chat ||
|#

## Continue Your Exploration

- [API imbot.v2 Change Log](../../change-log.md)
- [{#T}](../../index.md)
- [{#T}](../../entities.md)
- [{#T}](../../migration.md)
- [Bots imbot.v2](../bots/index.md)
- [imbot.v2 Messages](../messages/index.md)
- [Events imbot.v2](../events/index.md)