# Messages: Overview of Methods

This section describes the methods for sending, modifying, deleting messages, reading history, and working with reactions on behalf of a chat bot.

> Quick navigation: [all methods](#all-methods)

## Additional Messaging Features

When sending messages via [imbot.v2.Chat.Message.send](./chat-message-send.md), the following features are available:

- [Text Formatting (BB Codes)](./message-formatting.md)
- [Attachments](./attachments/index.md)
- [Keyboards](./message-keyboards.md)

## Overview of Methods {#all-methods}

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the methods: owner of the registered bot

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

## Continue Exploring

- [Chatbots 2.0: Overview of Methods](../../index.md)
- [imbot.v2 Files](../files/index.md)
- [Text Formatting (BB Codes)](./message-formatting.md)
- [Attachments in Messages ATTACH](./attachments/index.md)
- [Working with Keyboards](./message-keyboards.md)