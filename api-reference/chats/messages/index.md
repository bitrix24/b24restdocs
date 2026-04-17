# Messages: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so the assistant can utilize the official REST documentation.

{% endnote %}

The messaging methods allow you to send and modify messages, work with dialogue history, context menus, and service actions in chats.

> Quick navigation: [all methods](#all-methods)

## Messaging Capabilities

- [Formatting](./formatting.md) — which BB codes are supported in `MESSAGE` and how to use them to highlight text, add links, and command inserts
- [Attachments](./attachments.md) — how to create structured content `ATTACH`: blocks with text, links, files, and media elements
- [Keyboards](./keyboards.md) — how to add `KEYBOARD` with interactive buttons and handle user actions in messages
- [Context Menu](./menu.md) — how to configure `MENU` with additional actions and commands for a specific message

## Getting Started

1. Send a message using the [im.message.add](./im-message-add.md) method.
2. If necessary, modify or delete the message using the [im.message.update](./im-message-update.md) and [im.message.delete](./im-message-delete.md) methods.
3. Retrieve dialogue messages using the [im.dialog.messages.get](./im-dialog-messages-get.md) method.
4. Find the desired message using the [im.dialog.messages.search](./im-dialog-messages-search.md) method.
5. Manage the "read" status using the [im.dialog.read](./im-dialog-read.md), [im.dialog.unread](./im-dialog-unread.md) methods, and the "User is typing" indicator through [im.dialog.writing](./im-dialog-writing.md).

## Interaction with Other Objects

**User.** To send a message in a personal dialogue, specify the user ID in `DIALOG_ID` in the format `XXX`. You can obtain the user ID using the [user.get](../../user/user-get.md) and [user.search](../../user/user-search.md) methods.

**Chat.** To work with group chats, use `DIALOG_ID` in the format `chatXXX` or `sgXXX`. The `CHAT_ID` is used in message search methods. You can obtain the chat ID using the [im.chat.get](../im-chat-get.md) method.

**Chatbot.** To execute chatbot commands in the context of a message, use the [im.message.command](./im-message-command.md) method and pass `BOT_ID`, `COMMAND`. The bot ID can be obtained using the [imbot.bot.list](../../chat-bots/outdated/bots/imbot-bot-list.md) method.

## Overview of Methods {#all-methods}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the methods: depends on the method

#| 
|| **Method** | **Description** ||
|| [im.message.add](./im-message-add.md) | Adds a message to the chat ||
|| [im.message.update](./im-message-update.md) | Modifies the sent message ||
|| [im.message.delete](./im-message-delete.md) | Deletes a message ||
|| [im.message.like](./im-message-like.md) | Changes the "Like" status of a message ||
|| [im.message.share](./im-message-share.md) | Creates an object based on a message ||
|| [im.message.command](./im-message-command.md) | Executes a chatbot command ||
|| [im.dialog.messages.get](./im-dialog-messages-get.md) | Retrieves a list of recent messages ||
|| [im.dialog.messages.search](./im-dialog-messages-search.md) | Searches for a message in the chat ||
|| [im.dialog.read](./im-dialog-read.md) | Sets the "read" status for messages ||
|| [im.dialog.unread](./im-dialog-unread.md) | Sets the "unread" status for messages ||
|| [im.dialog.writing](./im-dialog-writing.md) | Sends the "User is typing" indicator ||
|#