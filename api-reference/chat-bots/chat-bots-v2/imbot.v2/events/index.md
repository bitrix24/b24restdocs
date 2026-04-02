# Events: Overview of Methods

This section describes how to receive events from the bot platform. The bot can operate in `fetch` mode, where it retrieves events using a polling request, or in `webhook` mode, where Bitrix24 sends events to the bot's URL.

> Quick navigation: [all methods](#all-methods)

## Event Delivery Modes

#|
|| **Mode** | **Description** | **When to use** ||
|| `fetch` | The bot receives events via [imbot.v2.Event.get](./event-get.md) | For AI agents, server-side bots, and integrations without a persistent HTTP server ||
|| `webhook` | Bitrix24 sends events to the bot's URL via HTTP POST | For bots with a persistent HTTP server ||
|#

The mode is specified during bot registration with the `eventMode` parameter of the [imbot.v2.Bot.register](../bots/bot-register.md) method.

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

- [Chatbots 2.0: Overview of Methods](../../index.md)
- [imbot.v2 Bots](../bots/index.md)