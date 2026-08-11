# Events: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The bot can operate in `fetch` mode, where it retrieves events through polling requests, or in `webhook` mode, where Bitrix24 sends events to the bot's URL.

> Quick navigation: [all methods and events](#all-methods)

## Event Delivery Modes {#event-modes}

#|
|| **Mode** | **Description** | **When to use** ||
|| `fetch` | The bot receives events via [imbot.v2.Event.get](./event-get.md) | For AI agents, server-side bots, and integrations without a persistent HTTP server ||
|| `webhook` | Bitrix24 sends events to the bot's URL via HTTP POST | For bots with a persistent HTTP server ||
|#

The mode is set during bot registration with the `eventMode` parameter of the [imbot.v2.Bot.register](../bots/bot-register.md) method.

The data format depends on the mode. In `fetch` mode, the full bot object arrives; in `webhook` mode, a simplified object `{id, code, auth}`. In webhook mode, all scalar values arrive as strings, so the types have to be cast explicitly. More details — [Format of the bot Object](./events.md#bot-format).

## Event Subscription {#subscription}

Subscriptions to `ONIMBOTV2*` events are managed by the platform itself:

- it creates the subscription on [imbot.v2.Bot.register](../bots/bot-register.md) with `eventMode: "webhook"`
- it rebuilds the subscription on [imbot.v2.Bot.update](../bots/bot-update.md) if `webhookUrl` or `eventMode` changes
- it removes the subscription on [imbot.v2.Bot.unregister](../bots/bot-unregister.md) or when the bot switches to `fetch` mode

{% note warning "" %}

There is no need to call `event.bind` and `event.unbind` manually — doing so may lead to a mismatch with the internal subscription records.

{% endnote %}

## Limits {#limits}

#|
|| **Limit** | **Value** ||
|| Events per single [imbot.v2.Event.get](./event-get.md) call | 1–1000, 100 by default ||
|| Recommended polling interval in `fetch` mode | 5–30 seconds when there are no new events ||
|| Delivery in `webhook` mode | The platform expects HTTP 200. Retries on failure are not guaranteed ||
|#

An example of a polling cycle and a webhook handler is available in [Chatbots 2.0: Overview of Methods](../../index.md#polling).

## Relationship with Other Objects {#relations}

**Bot.** Events are delivered to a specific bot, so `botId` is passed to [imbot.v2.Event.get](./event-get.md), and for webhook authorization, `botToken` as well — [Bots](../bots/index.md).

**Messages.** The `ONIMBOTV2MESSAGE*` and `ONIMBOTV2REACTIONCHANGE` events are the main incoming flow for the bot. Responses to them are sent by the methods of the [Messages](../messages/index.md) group.

**Commands.** A slash command call arrives as the `ONIMBOTV2COMMANDADD` event, and the response is sent by the [imbot.v2.Command.answer](../commands/command-answer.md) method — [Commands](../commands/index.md).

**Bot context.** The `ONIMBOTV2CONTEXTGET` event delivers to the bot the arbitrary data passed when the chat was opened — [Passing Context to a Bot](../bot-context.md).

**User events.** If you need to read the messenger event flow on behalf of a user rather than a bot, use the [im.v2.Event.*](../../im.v2/events/index.md) methods.

## Overview of Methods and Events {#all-methods}

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the methods: owner of the registered bot

### Methods

#|
|| **Method** | **Description** ||
|| [imbot.v2.Event.get](./event-get.md) | Returns bot events in polling mode ||
|| [Event Formats](./events.md) | Description of events and data structures ||
|#

### Events

The minimum set for a working bot is `ONIMBOTV2MESSAGEADD`, `ONIMBOTV2COMMANDADD`, `ONIMBOTV2JOINCHAT`, and `ONIMBOTV2DELETE`. Connect the remaining events as your scenario requires.

#|
|| **Event** | **Triggered** ||
|| [ONIMBOTV2MESSAGEADD](./events.md#onimbotv2messageadd) | When a user sends a message to a chat the bot belongs to ||
|| [ONIMBOTV2MESSAGEUPDATE](./events.md#onimbotv2messageupdate) | When a message is edited in the bot's chat ||
|| [ONIMBOTV2MESSAGEDELETE](./events.md#onimbotv2messagedelete) | When a message is deleted in the bot's chat ||
|| [ONIMBOTV2REACTIONCHANGE](./events.md#onimbotv2reactionchange) | When a reaction to the bot's message is added or removed ||
|| [ONIMBOTV2COMMANDADD](./events.md#onimbotv2commandadd) | When a user calls a slash command of the bot ||
|| [ONIMBOTV2JOINCHAT](./events.md#onimbotv2joinchat) | When the bot is added to a chat or invited to one ||
|| [ONIMBOTV2CONTEXTGET](./events.md#onimbotv2contextget) | When a dialog with the bot is opened, if context data is passed ||
|| [ONIMBOTV2DELETE](./events.md#onimbotv2delete) | When the bot is deleted from the system. The last event the bot receives ||
|#

## Continue Your Exploration

- [API Change Log for imbot.v2](../../change-log.md)
- [{#T}](../../index.md)
- [{#T}](../../entities.md)
- [{#T}](../../migration.md)
- [imbot.v2 Bots](../bots/index.md)
- [Commands imbot.v2](../commands/index.md)
- [{#T}](../bot-context.md)