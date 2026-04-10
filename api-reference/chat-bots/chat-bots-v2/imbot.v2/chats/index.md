# Chats: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

This section describes methods for creating group chats on behalf of a bot and managing participants, owners, and managers.

> Quick navigation: [all methods](#all-methods)

## What You Can Do with Chats

- create a new group chat
- retrieve chat data
- modify chat properties
- add and remove participants
- transfer ownership rights
- manage the list of managers

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
|| [imbot.v2.Chat.User.list](./chat-user-list.md) | Returns a list of chat participants ||
|| [imbot.v2.Chat.Manager.add](./chat-manager-add.md) | Adds chat managers ||
|| [imbot.v2.Chat.Manager.delete](./chat-manager-delete.md) | Removes chat managers ||
|| [imbot.v2.Chat.setOwner](./chat-set-owner.md) | Assigns a new owner to the chat ||
|#

## Continue Your Exploration

- [API Change Log for imbot.v2](../../change-log.md)
- [{#T}](../../index.md)
- [Messages for imbot.v2](../messages/index.md)