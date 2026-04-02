# Bots: Overview of Methods

This section covers the lifecycle methods of a chat bot: registration, property updates, data retrieval, list viewing, and deletion.

> Quick navigation: [all methods](#all-methods)

## How to Work with Bots

1. Register a bot using [imbot.v2.Bot.register](./bot-register.md).
2. If necessary, update properties with the [imbot.v2.Bot.update](./bot-update.md) method.
3. Retrieve data for a single bot using [imbot.v2.Bot.get](./bot-get.md) or a list using [imbot.v2.Bot.list](./bot-list.md).
4. Unregister the bot with the [imbot.v2.Bot.unregister](./bot-unregister.md) method if it is no longer needed.

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

- [Chatbots 2.0: Overview of Methods](../../index.md)
- [Chats imbot.v2](../chats/index.md)