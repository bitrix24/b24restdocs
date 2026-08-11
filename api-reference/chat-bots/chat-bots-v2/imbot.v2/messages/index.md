# Messages: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The group methods allow sending, modifying, and deleting messages, reading chat history, and working with reactions on behalf of the chatbot.

> Quick navigation: [all methods](#all-methods)

## How to Get Started {#how-to-start}

1. The bot receives an incoming message through the [ONIMBOTV2MESSAGEADD](../events/events.md#onimbotv2messageadd) event. It delivers `chat.dialogId` and `message.id`.
2. Send the response with the [imbot.v2.Chat.Message.send](./chat-message-send.md) method, passing the `dialogId` from the event. To make the response look like a reply, specify `fields.replyId`.
3. If necessary, modify the sent message with [imbot.v2.Chat.Message.update](./chat-message-update.md) or delete it with [imbot.v2.Chat.Message.delete](./chat-message-delete.md).
4. Mark the handled messages as read with the [imbot.v2.Chat.Message.read](./chat-message-read.md) method — it also returns the remaining unread counter.

Bots of the `supervisor` and `personal` types can additionally read other users' messages: [imbot.v2.Chat.Message.get](./chat-message-get.md) reads a single message by ID, and [imbot.v2.Chat.Message.getContext](./chat-message-get-context.md) returns the window of messages around it.

The description of the Message object fields is available in [Objects and Fields](../../entities.md#message).

## Additional Messaging Features {#extras}

When sending messages via [imbot.v2.Chat.Message.send](./chat-message-send.md), the following features are available:

- [Text Formatting (BB Codes)](./message-formatting.md) — bold, italic, links, quotes, and code in the message text
- [Attachments](./attachments/index.md) — structured blocks: text, links, images, files, tables
- [Keyboards](./message-keyboards.md) — interactive buttons under the message

## Limits {#limits}

#|
|| **Limit** | **Value** ||
|| Message text length | 20,000 characters. Any excess text is truncated ||
|| Messages per single `fields.forwardIds` forward | 100. Only messages from chats where the bot is a participant can be forwarded ||
|| Modification and deletion | The bot works only with its own messages. Only a bot that is a chat administrator can delete someone else's message ||
|| System messages with `fields.system: true` | They have `authorId = 0` and do not belong to the bot: they cannot be updated, and they can be deleted only on behalf of a chat administrator ||
|| Reading other users' messages | The `Chat.Message.get` and `Chat.Message.getContext` methods are available only to bots of the `supervisor` and `personal` types ||
|#

## Relationship with Other Objects {#relations}

**Bot.** Messages are sent on behalf of a registered bot: every call passes `botId`, and for webhook authorization, `botToken` as well — [Bots](../bots/index.md).

**Chats.** The recipient of a message is set by the `dialogId` parameter: for group chats it is `chat{chatId}`, for private chats it is the ID of the other participant. More details — [Format of dialogId](../../index.md#dialog-id) and [Chats](../chats/index.md).

**Events.** The incoming flow of messages, their edits, deletions, and reactions reaches the bot through the `ONIMBOTV2MESSAGE*` and `ONIMBOTV2REACTIONCHANGE` events — [Events](../events/index.md).

**Commands.** A response to a slash command is sent by the separate [imbot.v2.Command.answer](../commands/command-answer.md) method rather than `Chat.Message.send`. Only `Command.answer` can reply in a chat where the bot is not a participant — [Commands](../commands/index.md).

**Files.** The [imbot.v2.File.upload](../files/file-upload.md) method creates a message with the file itself, so sending a separate message is not needed — [Files](../files/index.md).

## Overview of Methods {#all-methods}

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute methods: owner of the registered bot

#| 
|| **Method** | **Description** ||
|| [imbot.v2.Chat.Message.send](./chat-message-send.md) | Sends a message in the chat ||
|| [imbot.v2.Chat.Message.update](./chat-message-update.md) | Updates the bot's message ||
|| [imbot.v2.Chat.Message.delete](./chat-message-delete.md) | Deletes a message ||
|| [imbot.v2.Chat.Message.read](./chat-message-read.md) | Marks messages as read ||
|| [imbot.v2.Chat.Message.get](./chat-message-get.md) | Returns a message by ID. For `supervisor` and `personal` only ||
|| [imbot.v2.Chat.Message.getContext](./chat-message-get-context.md) | Returns the message window surrounding the specified one. For `supervisor` and `personal` only ||
|| [imbot.v2.Chat.Message.Reaction.add](./chat-message-reaction-add.md) | Adds a reaction to a message ||
|| [imbot.v2.Chat.Message.Reaction.delete](./chat-message-reaction-delete.md) | Removes a reaction from a message ||
|#

## Continue Your Exploration

- [API imbot.v2 Change Log](../../change-log.md)
- [{#T}](../../index.md)
- [{#T}](../../entities.md)
- [{#T}](../../migration.md)
- [imbot.v2 Files](../files/index.md)
- [Events imbot.v2](../events/index.md)
- [{#T}](./message-formatting.md)
- [{#T}](./attachments/index.md)
- [{#T}](./message-keyboards.md)