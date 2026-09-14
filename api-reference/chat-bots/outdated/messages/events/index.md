# Overview of Events When Working with Messages

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% note warning "" %}

**DEPRECATED**

The development of events in the old section has been halted.  
Please use [Events `imbot.v2`](../../../chat-bots-v2/imbot.v2/events/events.md).

{% endnote %}

> Scope: [`imbot`](../../../../scopes/permissions.md)  
> Who can subscribe: any user

## Events

#|
|| **Event** | **Triggered By** ||
|| [ONIMBOTMESSAGEADD](./on-imbot-message-add.md) | When a message is sent in a chat with the bot manually or via the [imbot.message.add](../imbot-message-add.md) method ||
|| [ONIMBOTMESSAGEUPDATE](./on-imbot-message-update.md) | When a message is edited in a chat with the bot manually or via the [imbot.message.update](../imbot-message-update.md) method ||
|| [ONIMBOTMESSAGEDELETE](./on-imbot-message-delete.md) | When a message is deleted in a chat with the bot manually or via the [imbot.message.delete](../imbot-message-delete.md) method ||
|#