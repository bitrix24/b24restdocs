# Working with Messages

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% note warning "" %}

**DEPRECATED**

The development of the methods `imbot.message.*` has been halted.  
Please use the section [Messages (`imbot.v2.Chat.Message.*`)](../../chat-bots-v2/imbot.v2/messages/index.md).

{% endnote %}

> Scope: [`imbot`](../../../scopes/permissions.md)  
> Who can execute the method: any user

## Methods

#|
|| **Method** | **Description** ||
|| [imbot.message.add](./imbot-message-add.md) | Adds a new message from the chat bot ||
|| [imbot.message.update](./imbot-message-update.md) | Updates an existing message from the chat bot ||
|| [imbot.message.delete](./imbot-message-delete.md) | Deletes a message from the chat bot ||
|| [imbot.message.like](./imbot-message-like.md) | Likes a message from the chat bot ||
|#

## Events

#|
|| **Event** | **Triggered** ||
|| [ONIMBOTMESSAGEADD](./events/on-imbot-message-add.md) | When a message is sent ||
|| [ONIMBOTMESSAGEUPDATE](./events/on-imbot-message-update.md) | When a message from the chat bot is updated ||
|| [ONIMBOTMESSAGEDELETE](./events/on-imbot-message-delete.md) | When a message from the chat bot is deleted ||
|#