# Chats: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The methods allow you to create group chats on behalf of a bot and manage participants, the owner, and managers. They replace the `imbot.chat.*` methods of the first API version. The mapping of old and new methods is available in the [migration table](../../migration.md).

> Quick navigation: [all methods](#all-methods)

## How to Get Started {#how-to-start}

1. Create a group chat with the [imbot.v2.Chat.add](./chat-add.md) method.
2. Add participants with [imbot.v2.Chat.User.add](./chat-user-add.md) or pass them right away in `fields.userIds` when creating the chat.
3. If necessary, modify the chat properties with [imbot.v2.Chat.update](./chat-update.md). You can assign managers with the [imbot.v2.Chat.Manager.add](./chat-manager-add.md) method and remove them with [imbot.v2.Chat.Manager.delete](./chat-manager-delete.md).
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
|| **Role** | **Methods Available to a Bot with This Role by Default** | **How to Assign** ||
|| Owner | [imbot.v2.Chat.update](./chat-update.md), [imbot.v2.Chat.setOwner](./chat-set-owner.md), [imbot.v2.Chat.Manager.add](./chat-manager-add.md), [imbot.v2.Chat.Manager.delete](./chat-manager-delete.md), and all manager methods | `fields.ownerId` in [imbot.v2.Chat.add](./chat-add.md) or [imbot.v2.Chat.setOwner](./chat-set-owner.md) ||
|| Manager | [imbot.v2.Chat.User.delete](./chat-user-delete.md) and all participant methods | [imbot.v2.Chat.Manager.add](./chat-manager-add.md) ||
|| Participant | [imbot.v2.Chat.get](./chat-get.md), [imbot.v2.Chat.User.list](./chat-user-list.md), [imbot.v2.Chat.User.add](./chat-user-add.md), [imbot.v2.Chat.leave](./chat-leave.md) | `fields.userIds` in [imbot.v2.Chat.add](./chat-add.md) or [imbot.v2.Chat.User.add](./chat-user-add.md) ||
|#

The minimum role for each action is set in the settings of a specific chat. The exceptions are [imbot.v2.Chat.update](./chat-update.md), which always requires the owner role, and [imbot.v2.Chat.get](./chat-get.md), [imbot.v2.Chat.User.list](./chat-user-list.md), and [imbot.v2.Chat.leave](./chat-leave.md), which are available to any chat participant. The current values are returned in the `permissions` field of the [imbot.v2.Chat.get](./chat-get.md) response. If the bot lacks the required role, the method returns the `ACCESS_DENIED` error.

## Relationship with Other Objects {#relations}

The chat is linked to the bot on whose behalf the methods run, to messages and files, to events, to the typing indicator, and to users.

**Bot.** All methods of this section are executed on behalf of a registered bot: every call passes `botId`, and for webhook authorization, `botToken` as well. `botId` is returned by the bot registration method, and you set `botToken` yourself in `fields.botToken` during registration — [Bots](../bots/index.md).

**Messages and Files.** The chat is the recipient of the bot's messages. The chat identifier in the `dialogId` format is passed to the methods of the [Messages](../messages/index.md) and [Files](../files/index.md) groups.

**Events.** When the bot is added to a group chat, it receives the [ONIMBOTV2JOINCHAT](../events/events.md#onimbotv2joinchat) event. A typical reaction to it is to send a welcome message to the chat.

**Typing Indicator.** While the bot is preparing a response, you can show the “typing” status in the chat with the [imbot.v2.Chat.InputAction.notify](../ui/chat-input-action-notify.md) method — it takes the same chat `dialogId`.

**Users.** Participants and managers are set as arrays of Bitrix24 user IDs. The description of the User object fields is available in [Objects and Fields](../../entities.md#user).

## Overview of Methods {#all-methods}

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the methods: owner of the registered bot

### Chat

#| 
|| **Method** | **Description** ||
|| [imbot.v2.Chat.add](./chat-add.md) | Creates a group chat ||
|| [imbot.v2.Chat.update](./chat-update.md) | Updates chat properties ||
|| [imbot.v2.Chat.get](./chat-get.md) | Returns information about the chat ||
|| [imbot.v2.Chat.setOwner](./chat-set-owner.md) | Assigns a new owner to the chat ||
|| [imbot.v2.Chat.leave](./chat-leave.md) | Removes the bot from the chat ||
|#

### Participants

#|
|| **Method** | **Description** ||
|| [imbot.v2.Chat.User.add](./chat-user-add.md) | Adds participants to the chat ||
|| [imbot.v2.Chat.User.list](./chat-user-list.md) | Returns the list of chat participants ||
|| [imbot.v2.Chat.User.delete](./chat-user-delete.md) | Removes a participant from the chat ||
|#

### Managers

#|
|| **Method** | **Description** ||
|| [imbot.v2.Chat.Manager.add](./chat-manager-add.md) | Adds chat managers ||
|| [imbot.v2.Chat.Manager.delete](./chat-manager-delete.md) | Removes chat managers ||
|#

## Continue Your Exploration

- [API imbot.v2 Change Log](../../change-log.md)
- [{#T}](../../index.md)
- [{#T}](../../entities.md)
- [{#T}](../../migration.md)
- [Bots imbot.v2](../bots/index.md)
- [imbot.v2 Messages](../messages/index.md)
- [Events imbot.v2](../events/index.md)