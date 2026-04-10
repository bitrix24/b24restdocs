# Chat Bot Chats: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

{% note warning "" %}

**DEPRECATED**

The development of methods `imbot.chat.*` has been halted.  
Please use the section [Chats (`imbot.v2.Chat.*`)](../../chat-bots-v2/imbot.v2/chats/index.md).

{% endnote %}

> Scope: [`imbot`](../../../scopes/permissions.md)  
> Who can execute the method: depends on the method

## Methods

#|
|| **Method** | **Description** ||
|| [imbot.chat.add](./imbot-chat-add.md) | Creates a new chat ||
|| [imbot.chat.get](./imbot-chat-get.md) | Returns information about the chat ||
|| [imbot.chat.leave](./imbot-chat-leave.md) | Executes the chat bot's exit from the specified chat ||
|| [imbot.chat.setManager](./imbot-chat-set-manager.md) | Assigns or removes admin rights from a chat participant ||
|| [imbot.chat.sendTyping](./imbot-chat-send-typing.md) | Sends a typing indicator to the chat ||
|| [imbot.chat.updateAvatar](./imbot-chat-update-avatar.md) | Updates the chat avatar ||
|| [imbot.chat.updateColor](./imbot-chat-update-color.md) | Updates the chat color ||
|| [imbot.chat.updateTitle](./imbot-chat-update-title.md) | Updates the chat title ||
|| [imbot.chat.user.add](./imbot-chat-user-add.md) | Adds a user to the chat ||
|| [imbot.chat.user.list](./imbot-chat-user-list.md) | Returns the list of users in the chat ||
|| [imbot.chat.user.delete](./imbot-chat-user-delete.md) | Removes a user from the chat ||
|| [imbot.dialog.get](./imbot-dialog-get.md) | Returns information about the dialog ||
|#

## Events

#|
|| **Event** | **Triggered** ||
|| [ONIMBOTJOINCHAT](./events/on-imbot-join-chat.md) | When the chat bot receives information about being added to a chat (or personal conversation) ||
|#