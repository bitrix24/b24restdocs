# Bots: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The methods manage the lifecycle of a chat bot: registration, property modification, data retrieval, list viewing, and deletion.

> Quick navigation: [all methods](#all-methods)

## How to Work with Bots

1. Register the bot using [imbot.v2.Bot.register](./bot-register.md). The response returns `botId` — it is required in all other `imbot.v2` calls.
2. If necessary, update the properties using [imbot.v2.Bot.update](./bot-update.md).
3. Retrieve data for a single bot using [imbot.v2.Bot.get](./bot-get.md) or a list using [imbot.v2.Bot.list](./bot-list.md).
4. Delete the bot using [imbot.v2.Bot.unregister](./bot-unregister.md) if it is no longer needed.

A full description of the Bot object fields is available in [Objects and Fields](../../entities.md#bot).

## Key Bot Parameters {#key-fields}

These parameters are set during registration and determine how the bot will operate afterwards.

#|
|| **Parameter** | **What It Determines** | **Where It Is Described in Detail** ||
|| `fields.code` | The unique code of the bot within the application. It allows `imbot.v2.Bot.register` to recognize a repeated registration and return the existing bot | [imbot.v2.Bot.register](./bot-register.md) ||
|| `fields.botToken` | The authorization token of the bot. Required for webhook calls, not needed for OAuth. Maximum length — 40 characters | [Authorization](../../index.md#auth) ||
|| `fields.type` | The behavior of the bot in chats: `bot`, `supervisor`, `personal`, `openline`. The type determines whether the bot receives all chat messages or only mentions | [Bot Types](../../index.md#bot-types) ||
|| `fields.eventMode` | The event delivery method: `fetch` — the bot retrieves events itself, `webhook` — Bitrix24 sends them to the bot's URL | [Event Delivery Modes](../events/index.md) ||
|#

Limit: a single application can register no more than 100 bots. The other limits of the section are listed in [Limits](../../index.md#limits).

## Relationship with Other Objects {#relations}

**Events.** The bot receives `ONIMBOTV2*` events in the mode set in `fields.eventMode`. The event subscription is created automatically when the bot is registered and cleared when it is deleted — there is no need to call `event.bind` manually. Data formats are described in [Events imbot.v2](../events/events.md).

**Chats.** A registered bot can create group chats and manage participants. All such calls pass the `botId` from the `imbot.v2.Bot.register` response — [Chats](../chats/index.md).

**Commands.** Slash commands are registered separately and linked to the bot by `botId` — [Commands](../commands/index.md).

**Open Channels.** The `fields.isSupportOpenline` property enables Open Channels support for a bot of the `openline` type — [imbot.v2.Bot.register](./bot-register.md).

**Bot context.** An application can pass arbitrary JSON data to the bot when a chat is opened — [Passing Context to a Bot](../bot-context.md).

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
- [{#T}](../../entities.md)
- [{#T}](../../migration.md)
- [Chats imbot.v2](../chats/index.md)
- [Events imbot.v2](../events/index.md)
- [{#T}](../bot-context.md)