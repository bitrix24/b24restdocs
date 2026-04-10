# Events: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

This section describes how to retrieve events from the bot platform. The bot can operate in `fetch` mode, where it retrieves events via polling requests, or in `webhook` mode, where Bitrix24 sends events to the bot's URL.

> Quick navigation: [all methods](#all-methods)

## Event Delivery Modes

#| 
|| **Mode** | **Description** | **When to use** ||
|| `fetch` | The bot receives events through [imbot.v2.Event.get](./event-get.md) | For AI agents, server-side bots, and integrations without a persistent HTTP server ||
|| `webhook` | Bitrix24 sends events to the bot's URL via HTTP POST | For bots with a persistent HTTP server ||
|#

The mode is specified during bot registration using the `eventMode` parameter of the [imbot.v2.Bot.register](../bots/bot-register.md) method.

## Overview of Methods {#all-methods}

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the methods: owner of the registered bot

#| 
|| **Method** | **Description** ||
|| [imbot.v2.Event.get](./event-get.md) | Returns bot events in polling mode ||
|| [Event Formats](./events.md) | Description of events and data structures ||
|#

## Continue Your Exploration

- [API Change Log for imbot.v2](../../change-log.md)
- [{#T}](../../index.md)
- [imbot.v2 Bots](../bots/index.md)