# Messages: Overview of Methods

This section consolidates methods for sending and modifying messages, managing chat history, context menus, and service actions in chats.

> Quick Navigation: [All Methods](#all-methods)

## Messaging Capabilities

- [Formatting](./formatting.md) — BB codes in message text
- [Attachments](./attachments.md) — structured content `ATTACH`
- [Keyboards](./keyboards.md) — interactive buttons `KEYBOARD`
- [Context Menu](./menu.md) — commands and actions in the message menu

## Overview of Methods {#all-methods}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute methods: a user with access to the chat

#|
|| **Method** | **Description** ||
|| [im.message.add](./im-message-add.md) | Adds a message to the chat ||
|| [im.message.update](./im-message-update.md) | Modifies a sent message ||
|| [im.message.delete](./im-message-delete.md) | Deletes a message ||
|| [im.message.like](./im-message-like.md) | Changes the "Like" status of a message ||
|| [im.message.share](./im-message-share.md) | Creates an object based on a message ||
|| [im.message.command](./im-message-command.md) | Executes a chatbot command ||
|| [im.dialog.messages.get](./im-dialog-messages-get.md) | Retrieves a list of recent messages ||
|| [im.dialog.messages.search](./im-dialog-messages-search.md) | Searches for a message in the chat ||
|| [im.dialog.read](./im-dialog-read.md) | Marks messages as "read" ||
|| [im.dialog.unread](./im-dialog-unread.md) | Marks messages as "unread" ||
|| [im.dialog.writing](./im-dialog-writing.md) | Sends the "User is typing" status ||
|#

## Continue Your Exploration

- [{#T}](./formatting.md)
- [{#T}](./attachments.md)
- [{#T}](./keyboards.md)
- [{#T}](./menu.md)