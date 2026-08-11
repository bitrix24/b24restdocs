# Messages: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so the assistant can utilize the official REST documentation.

{% endnote %}

The messaging methods send and modify messages, read the dialogue history, manage the read status, and handle the context menu of a message.

A message is sent to a dialogue, not to a chat: the recipient is set by the `DIALOG_ID` parameter. The same set of methods works both for a private conversation and for a group chat — only the value of `DIALOG_ID` changes. The subsection is part of the [Chats in Bitrix24](../index.md) section.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Chats in Bitrix24: Interface and Capabilities](https://helpdesk.bitrix24.com/open/21924784/)

## Dialogue Identifier

`DIALOG_ID` determines where the message goes:

#|
|| **Format** | **Where It Is Sent** | **Example** ||
|| `XXX` | A private dialogue with a user, where `XXX` is the user identifier | `47` ||
|| `chatXXX` | A group chat, where `XXX` is the chat identifier | `chat2935` ||
|| `sgXXX` | A workgroup or project chat, where `XXX` is the group identifier | `sg17` ||
|#

In the message search methods, a numeric `CHAT_ID` is used instead of `DIALOG_ID` — this is the chat identifier without the `chat` prefix.

## Limits and Response Format

- the size of the serialized `ATTACH` attachment is limited to 60,000 characters, and `KEYBOARD` and `MENU` have a limit as well. The error codes returned when the limit is exceeded differ between the send and update methods — see the "Error Handling" section on the page of the required method. The complete attachment reference is in the [Attachments imbot.v2](../../chat-bots/chat-bots-v2/imbot.v2/messages/attachments/index.md) section
- the history-reading methods return messages page by page: the page size and its maximum differ, and the exact values are in the parameters of the required method
- editing and deleting a message are limited in time: the period is set by the Bitrix24 settings, and after it expires the methods return the `CANT_EDIT_MESSAGE` error
- the response fields are named differently, and one method cannot be generalized to the whole subsection:
    - [im.dialog.messages.get](./im-dialog-messages-get.md) returns `snake_case`: `chat_id`, `author_id`
    - [im.dialog.read](./im-dialog-read.md) returns `camelCase`: `dialogId`, `chatId`, `lastId`, `counter`
    - [im.dialog.messages.search](./im-dialog-messages-search.md) returns `camelCase` at the top level, while inside the message object it returns both `chatId` and `chat_id`, `authorId` and `author_id`

## Relevance of the Methods

The `im.message.*` and `im.dialog.*` methods are current and have no deprecated variants. They send messages on behalf of a user. Two adjacent tasks are handled outside this subsection:

- messages on behalf of a chatbot are sent by the methods of the [Messages imbot.v2](../../chat-bots/chat-bots-v2/imbot.v2/messages/index.md) subsection
- the scenario of opening an application from the message context menu is deprecated; use [messenger widgets](../../widgets/im/index.md) instead

## Messaging Capabilities

The current references for the message content are collected in the Chatbots 2.0 section — all the fields are listed there:

- [Formatting imbot.v2](../../chat-bots/chat-bots-v2/imbot.v2/messages/message-formatting.md) — BB codes in `MESSAGE`
- [Attachments imbot.v2](../../chat-bots/chat-bots-v2/imbot.v2/messages/attachments/index.md) — `ATTACH` blocks
- [Keyboards imbot.v2](../../chat-bots/chat-bots-v2/imbot.v2/messages/message-keyboards.md) — `KEYBOARD` buttons

Examples of these structures in the context of the `im.message.*` methods are collected on the [Formatting](./formatting.md), [Attachments](./attachments.md), [Keyboards](./keyboards.md), and [Context Menu](./menu.md) pages.

## Getting Started

1. Send a message using the [im.message.add](./im-message-add.md) method.
2. If necessary, modify or delete the message using the [im.message.update](./im-message-update.md) and [im.message.delete](./im-message-delete.md) methods.
3. Retrieve dialogue messages using the [im.dialog.messages.get](./im-dialog-messages-get.md) method.
4. Find the desired message using the [im.dialog.messages.search](./im-dialog-messages-search.md) method.
5. Manage the "read" status using the [im.dialog.read](./im-dialog-read.md), [im.dialog.unread](./im-dialog-unread.md) methods, and the "User is typing" indicator through [im.dialog.writing](./im-dialog-writing.md).

## Interaction with Other Objects

**User.** To send a message in a personal dialogue, specify the user ID in `DIALOG_ID` in the format `XXX`. You can obtain the user ID using the [user.get](../../user/user-get.md) and [user.search](../../user/user-search.md) methods.

**Chat.** The identifier of a group chat is returned by the [im.chat.get](../im-chat-get.md) method. The title, color, avatar, and owner of a chat are changed by the methods of the [Chat Update](../chat-update/index.md) subsection.

**Chatbot.** To execute chatbot commands in the context of a message, use the [im.message.command](./im-message-command.md) method and pass `BOT_ID`, `COMMAND`. The list of bots is returned by the [imbot.v2.Bot.list](../../chat-bots/chat-bots-v2/imbot.v2/bots/bot-list.md) method — it is called with a bot token.

**Files.** A file cannot be sent to a chat with a separate [im.message.add](./im-message-add.md) call: the file is uploaded by the [im.v2.File.upload](../../chat-bots/chat-bots-v2/im.v2/files/file-upload.md) method, which also creates the message. Details are in the [Files](../files/index.md) subsection.

**Chat participants.** A message is seen by the chat participants, so the recipient has to be added to the chat first. The composition is managed by the methods of the [Chat Participants](../chat-users/index.md) subsection.

**Notifications.** If a message should not appear in the conversation, send a notification with the methods of the [Notifications](../notifications/index.md) subsection.

**Chat list.** The [im.dialog.read](./im-dialog-read.md) and [im.dialog.unread](./im-dialog-unread.md) methods work with the read status inside a dialogue. You can mark all the user's chats as read at once with the [im.dialog.read.all](../special-operations/im-dialog-read-all.md) method, and the labels at the chat list level are managed by the methods of the [Special Operations](../special-operations/index.md) subsection.

## Overview of Methods {#all-methods}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

### Message

#|
|| **Method** | **Description** ||
|| [im.message.add](./im-message-add.md) | Adds a message to the chat ||
|| [im.message.update](./im-message-update.md) | Modifies the sent message ||
|| [im.message.delete](./im-message-delete.md) | Deletes a message ||
|| [im.message.like](./im-message-like.md) | Changes the "Like" status of a message ||
|| [im.message.share](./im-message-share.md) | Creates a chat, task, post, or calendar event based on a message ||
|| [im.message.command](./im-message-command.md) | Executes a chatbot command ||
|#

### Dialogue

#|
|| **Method** | **Description** ||
|| [im.dialog.messages.get](./im-dialog-messages-get.md) | Retrieves a list of recent messages ||
|| [im.dialog.messages.search](./im-dialog-messages-search.md) | Searches for a message in the chat ||
|| [im.dialog.read](./im-dialog-read.md) | Sets the "read" status for messages ||
|| [im.dialog.unread](./im-dialog-unread.md) | Sets the "unread" status for messages ||
|| [im.dialog.writing](./im-dialog-writing.md) | Sends the "User is typing" indicator ||
|#