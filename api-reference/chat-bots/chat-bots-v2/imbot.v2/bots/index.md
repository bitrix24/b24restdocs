# Bots: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The methods manage the lifecycle of a chat bot: registration, property modification, data retrieval, list viewing, and deletion.

> Quick navigation: [all methods](#all-methods)

## Migration from imbot to imbot.v2

The version 2 methods replace the deprecated [imbot.register](../../../outdated/bots/imbot-register.md), [imbot.update](../../../outdated/bots/imbot-update.md), [imbot.unregister](../../../outdated/bots/imbot-unregister.md), and [imbot.bot.list](../../../outdated/bots/imbot-bot-list.md). The [imbot.v2.Bot.get](./bot-get.md) method has no counterpart in the first version. For a new integration, choose the second version.

If the bot already uses the old methods, map the parameters using the [Migration Table](../../migration.md#methods): for example, `CODE` becomes `fields.code`, and instead of `EVENT_HANDLER` you set `fields.eventMode` and `fields.webhookUrl`. Both versions work in parallel, but the bot receives events in the format of the version through which it was registered.

## How to Work with Bots

1. Register the bot using [imbot.v2.Bot.register](./bot-register.md). Retain the bot ID from the `result.bot.id` response field. It is passed in the `botId` parameter to methods that work with a specific bot: updating and deleting the bot, and working with its chats, messages, and commands.
2. If necessary, update the properties using [imbot.v2.Bot.update](./bot-update.md).
3. Retrieve data for a single bot using [imbot.v2.Bot.get](./bot-get.md) or a list using [imbot.v2.Bot.list](./bot-list.md).
4. Delete the bot using [imbot.v2.Bot.unregister](./bot-unregister.md) if it is no longer needed.

A full description of the Bot object fields is available in [Objects and Fields](../../entities.md#bot).

## Key Bot Parameters {#key-fields}

These parameters are set during registration and determine how the bot will operate afterwards.

#|
|| **Parameter** | **What It Determines** | **Where It Is Described in Detail** ||
|| `fields.code` | The unique code of the bot within the application. It allows [imbot.v2.Bot.register](./bot-register.md) to recognize a repeated registration and return the existing bot | [imbot.v2.Bot.register](./bot-register.md) ||
|| `fields.botToken` | The authorization token of the bot. Required for webhook calls, not needed for OAuth. During registration, it is passed in `fields`; in other methods, at the top level of the request as `botToken`. Use a token of no more than 40 characters: this limit applies when the token is changed | [Authorization](../../index.md#auth) ||
|| `fields.type` | The behavior of the bot in chats, `bot` by default. The type determines whether the bot receives all chat messages or only mentions | [Bot Types](../../index.md#bot-types) ||
|| `fields.eventMode` | The event delivery method: `fetch` — the bot retrieves events itself, `webhook` — Bitrix24 sends them to the bot's URL | [Event Delivery Modes](../events/index.md) ||
|#

## Limits and Errors {#errors}

Limit: a single application can register no more than 100 bots. The other limits of the section are listed in [Limits](../../index.md#limits).

If a call fails, the method returns an error code in the `error` field. Typical errors:

- `BOT_TOKEN_NOT_SPECIFIED` — the request is sent via a webhook without `botToken`
- `BOT_NOT_FOUND` — there is no bot with the passed `botId`
- `BOT_OWNERSHIP_ERROR` — the bot was registered by another application or by a webhook with a different `botToken`

The error response structure is described in [Response Format](../../index.md#response-format), and the full list of codes is on the method pages.

## Relationship with Other Objects {#relations}

**Events.** The bot receives `ONIMBOTV2*` events in the mode set in `fields.eventMode`. In `fetch` mode, the bot retrieves events itself with the [imbot.v2.Event.get](../events/event-get.md) method. In `webhook` mode, Bitrix24 subscribes the bot to events during registration and removes the subscriptions together with the bot — there is no need to call [event.bind](../../../../events/event-bind.md) manually. If the application still has other bots, the subscriptions are retained until the last of them is deleted — see [imbot.v2.Bot.unregister](./bot-unregister.md#event-subscriptions) for details. Data formats are described in [Events imbot.v2](../events/events.md).

**Chats.** A registered bot can create group chats and manage participants. All such calls pass the `botId` from the [imbot.v2.Bot.register](./bot-register.md) response — [Chats](../chats/index.md).

**Commands.** Slash commands are registered separately and linked to the bot by `botId` — [Commands](../commands/index.md).

**Open Channels.** The `fields.isSupportOpenline` property enables Open Channels support for a bot of the `openline` type — [imbot.v2.Bot.register](./bot-register.md).

**Bot context.** An application can pass arbitrary JSON data to the bot when a chat is opened — [Passing Context to a Bot](../bot-context.md).

## Overview of Methods {#all-methods}

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the methods: an authorized user can register a bot and retrieve bot data and the list of bots; the bot owner can update and delete the bot

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