# Messages: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Messages are conversations in Bitrix24 chats: private conversations with colleagues, group chats, and project chats. The methods of this group send, modify, and delete messages, read the conversation history, mark messages as read, and add custom context menu items to a message. For example, an app can notify the responsible person in a private dialogue that a new request has arrived. The subsection is part of the [Chats in Bitrix24](../index.md) section.

The `im.message.*` and `im.dialog.*` methods are current and have no deprecated variants.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Chats in Bitrix24: Interface and Capabilities](https://helpdesk.bitrix24.com/open/21924784/)

## Dialogue Identifier

The methods call any conversation, private or group, a dialogue and handle it the same way. Only the value of `DIALOG_ID` differs:

#|
|| **Format** | **Dialogue** | **Example** ||
|| `XXX` | A private dialogue with a user, where `XXX` is the user identifier | `47` ||
|| `chatXXX` | A group chat, where `XXX` is the chat identifier | `chat2935` ||
|| `sgXXX` | A group or project chat, where `XXX` is the group identifier | `sg17` ||
|#

The [im.dialog.messages.search](./im-dialog-messages-search.md) method accepts a numeric `CHAT_ID` instead of `DIALOG_ID` — the chat identifier without the `chat` prefix: for `chat2935`, it is `2935`.

The [im.dialog.writing](./im-dialog-writing.md) method does not support the `sgXXX` format: for a workgroup chat, pass the identifier in the `chatXXX` format.

## Message, Notification, or Bot

#|
|| **Task** | **What to Use** ||
|| Write to a dialogue or a group chat on behalf of a user | The `im.message.*` and `im.dialog.*` methods of this subsection ||
|| Write on behalf of a chatbot | The methods of the [Messages imbot.v2](../../chat-bots/chat-bots-v2/imbot.v2/messages/index.md) subsection ||
|| Inform a user about an event without creating a conversation | The methods of the [Notifications](../notifications/index.md) subsection ||
|| Open an app from a message | [Messenger widgets](../../widgets/im/index.md). The message context menu item that opens an app by `APP_ID` is deprecated ||
|#

## Getting Started

1. Send a message using the [im.message.add](./im-message-add.md) method.
2. If necessary, modify or delete the message using the [im.message.update](./im-message-update.md) and [im.message.delete](./im-message-delete.md) methods.
3. Retrieve dialogue messages using the [im.dialog.messages.get](./im-dialog-messages-get.md) method.
4. Find the desired message using the [im.dialog.messages.search](./im-dialog-messages-search.md) method.
5. Mark messages as read or unread using the [im.dialog.read](./im-dialog-read.md) and [im.dialog.unread](./im-dialog-unread.md) methods.

## Relationship of Messages with Other Objects

**User.** To write to a user in a private dialogue, pass the user identifier in `DIALOG_ID`, for example, `47`. The identifier is returned by the [user.get](../../user/user-get.md) and [user.search](../../user/user-search.md) methods.

**Chat.** The identifier of a group chat is returned by the [im.chat.get](../im-chat-get.md) method. The title, color, avatar, and owner of a chat are changed by the methods of the [Chat Update](../chat-update/index.md) subsection.

**Chat participants.** A message is seen by the chat participants, so the recipient has to be added to the chat first. Chat membership is managed by the methods of the [Chat Participants](../chat-users/index.md) subsection.

**Chatbot.** A chatbot command in the context of a message is executed by the [im.message.command](./im-message-command.md) method: pass `MESSAGE_ID`, `BOT_ID`, and `COMMAND` to it. The list of bots is returned by the [imbot.v2.Bot.list](../../chat-bots/chat-bots-v2/imbot.v2/bots/bot-list.md) method.

**Message content.** Examples of text formatting, attachments, buttons, and menu items for the `im.message.*` methods are on the [Formatting](./formatting.md), [Attachments](./attachments.md), [Keyboards](./keyboards.md), and [Context Menu](./menu.md) pages. The complete field references are in the Chatbots 2.0 section: [Formatting imbot.v2](../../chat-bots/chat-bots-v2/imbot.v2/messages/message-formatting.md), [Attachments imbot.v2](../../chat-bots/chat-bots-v2/imbot.v2/messages/attachments/index.md), [Keyboards imbot.v2](../../chat-bots/chat-bots-v2/imbot.v2/messages/message-keyboards.md).

**Files.** A file is sent to a chat by the [im.v2.File.upload](../../chat-bots/chat-bots-v2/im.v2/files/file-upload.md) method: it uploads the file and creates the message itself. The [im.message.add](./im-message-add.md) method cannot send a file. Details are in the [Files](../files/index.md) subsection.

**Chat list.** The [im.dialog.read](./im-dialog-read.md) and [im.dialog.unread](./im-dialog-unread.md) methods manage the read status of messages in a single dialogue. You can mark all the user's chats as read at once with the [im.dialog.read.all](../special-operations/im-dialog-read-all.md) method. Labels in the chat list are managed by the methods of the [Special Operations](../special-operations/index.md) subsection.

## How to Choose a Method

#|
|| **Task** | **Method** ||
|| Send, edit, or delete a message | [im.message.add](./im-message-add.md), [im.message.update](./im-message-update.md), [im.message.delete](./im-message-delete.md). You can edit your own message within three days and delete it without a time limit ||
|| React to a message | [im.message.like](./im-message-like.md) — sets or removes the "Like" mark ||
|| Turn a message into a chat, task, post, or event | [im.message.share](./im-message-share.md). The method does not return the identifier of the created object. You can find the object by the system message in the chat ||
|| Execute a chatbot command from a message | [im.message.command](./im-message-command.md) ||
|| Retrieve the dialogue history | [im.dialog.messages.get](./im-dialog-messages-get.md) — returns chat messages. Earlier messages are loaded with `LAST_ID`, newer ones with `FIRST_ID` ||
|| Find messages by text or date | [im.dialog.messages.search](./im-dialog-messages-search.md) ||
|| Manage message read status | [im.dialog.read](./im-dialog-read.md) — marks messages as read up to the specified one, [im.dialog.unread](./im-dialog-unread.md) — marks messages as unread starting from the specified one ||
|| Show that the user is typing | [im.dialog.writing](./im-dialog-writing.md) ||
|#

## Method Responses

- the send method [im.message.add](./im-message-add.md) returns the identifier of the created message in the `result` field, type `integer`. The `MESSAGE_ID` parameters of other methods accept it
- the field names in the response differ between methods. [im.dialog.messages.get](./im-dialog-messages-get.md) returns fields in `snake_case`, for example, `chat_id`, [im.dialog.read](./im-dialog-read.md) — in `camelCase`, for example, `chatId`. [im.dialog.messages.search](./im-dialog-messages-search.md) returns both variants in the message object

## Overview of Methods {#all-methods}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#|
|| **Method** | **Description** ||
|| [im.message.add](./im-message-add.md) | Sends a message to the chat ||
|| [im.message.update](./im-message-update.md) | Modifies the sent message ||
|| [im.message.delete](./im-message-delete.md) | Deletes a message ||
|| [im.message.like](./im-message-like.md) | Sets or removes the "Like" mark ||
|| [im.message.share](./im-message-share.md) | Creates a chat, task, post, or calendar event from a message ||
|| [im.message.command](./im-message-command.md) | Executes a chatbot command ||
|| [im.dialog.messages.get](./im-dialog-messages-get.md) | Returns chat messages ||
|| [im.dialog.messages.search](./im-dialog-messages-search.md) | Searches for messages in the chat ||
|| [im.dialog.read](./im-dialog-read.md) | Marks messages as read ||
|| [im.dialog.unread](./im-dialog-unread.md) | Marks messages as unread ||
|| [im.dialog.writing](./im-dialog-writing.md) | Sends the "User is typing" indicator ||
|#