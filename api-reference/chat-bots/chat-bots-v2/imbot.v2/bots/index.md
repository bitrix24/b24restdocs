# Bots: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

This section covers the lifecycle methods of a chat bot: registration, property updates, data retrieval, list viewing, and deletion.

> Quick Navigation: [All Methods](#all-methods)

## How to Work with Bots

1. Register the bot using [imbot.v2.Bot.register](./bot-register.md).
2. If necessary, update the properties using [imbot.v2.Bot.update](./bot-update.md).
3. Retrieve data for a single bot using [imbot.v2.Bot.get](./bot-get.md) or a list using [imbot.v2.Bot.list](./bot-list.md).
4. Unregister the bot using [imbot.v2.Bot.unregister](./bot-unregister.md) if it is no longer needed.

## Overview of Methods {#all-methods}

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the methods: owner of the registered bot

#| 
|| **Method** | **Description** ||
|| [imbot.v2.Bot.register](./bot-register.md) | Registers a new bot ||
|| [imbot.v2.Bot.update](./bot-update.md) | Updates the properties of the bot ||
|| [imbot.v2.Bot.get](./bot-get.md) | Returns information about the bot ||
|| [imbot.v2.Bot.list](./bot-list.md) | Returns a list of the application's bots ||
|| [imbot.v2.Bot.unregister](./bot-unregister.md) | Deletes the bot ||
|#

## Continue Your Exploration

- [API Change Log for imbot.v2](../../change-log.md)
- [{#T}](../../index.md)
- [Chats in imbot.v2](../chats/index.md)