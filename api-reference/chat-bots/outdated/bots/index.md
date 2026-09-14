# Chatbots: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% note warning "" %}

**DEPRECATED**

The development of `imbot.*` methods has been halted.  
Please use the section on [Chatbots (`imbot.v2.Bot.*`)](../../chat-bots-v2/imbot.v2/bots/index.md).

{% endnote %}

> Scope: [`imbot`](../../../scopes/permissions.md)  
> Who can execute the method: depends on the method

## Methods

#|
|| **Method** | **Description** ||
|| [imbot.register](./imbot-register.md) | Registers a new chatbot ||
|| [imbot.update](./imbot-update.md) | Updates the chatbot's data and its event handlers ||
|| [imbot.bot.list](./imbot-bot-list.md) | Returns a list of registered chatbots ||
|| [imbot.unregister](./imbot-unregister.md) | Removes a chatbot ||
|#