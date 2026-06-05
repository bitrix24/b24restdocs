# Messages: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The group methods allow sending, modifying, and deleting messages, reading chat history, and working with reactions on behalf of the chatbot.

> Quick navigation: [all methods](#all-methods)

## Additional Messaging Features

When sending messages via [imbot.v2.Chat.Message.send](./chat-message-send.md), the following features are available:

- [Text Formatting (BB Codes)](./message-formatting.md)
- [Attachments](./attachments/index.md)
- [Keyboards](./message-keyboards.md)

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
- [imbot.v2 Files](../files/index.md)
- [{#T}](./message-formatting.md)
- [{#T}](./attachments/index.md)
- [{#T}](./message-keyboards.md)