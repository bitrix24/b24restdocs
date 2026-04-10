# Chatbots: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

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