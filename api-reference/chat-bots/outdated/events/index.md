# About Events

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

{% note warning "" %}

**DEPRECATED**

The development of the old Bot API has been discontinued.  
Please use [Events `imbot.v2`](../../chat-bots-v2/imbot.v2/events/index.md).

{% endnote %}

## Events

#|
|| **Event** | **Triggered By** ||
|| [ONAPPINSTALL](../../../common/events/on-app-install.md) | Event triggered when an application with a chat bot is installed ||
|| [ONAPPUPDATE](../../../common/events/on-app-update.md) | Event triggered when an application is updated ||
|| [ONIMBOTMESSAGEADD](../messages/events/on-imbot-message-add.md) | Event triggered after a message is sent from a user to the chat bot ||
|| [ONIMBOTMESSAGEUPDATE](../messages/events/on-imbot-message-update.md) | Event triggered after a message from the chat bot is updated ||
|| [ONIMBOTMESSAGEDELETE](../messages/events/on-imbot-message-delete.md) | Event triggered after a message from the chat bot is deleted ||
|| [ONIMCOMMANDADD](../commands/events/on-im-command-add.md) | Event triggered after a command is sent from a user to the chat bot ||
|| [ONIMBOTJOINCHAT](../chats/events/on-imbot-join-chat.md) | Event triggered when the chat bot joins a chat or personal dialogue ||
|| [ONIMBOTDELETE](./on-imbot-delete.md) | Event triggered after the application is deleted ||
|#