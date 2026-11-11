# Overview of imbot.v2 Methods and Events

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Events tell the bot what is happening in chats: a user wrote to the bot, called a command, or added the bot to a chat. The bot can collect events itself — `fetch` mode, or receive them from Bitrix24 at its URL — `webhook` mode.

> Quick navigation: [all methods and events](#all-methods)

## Event Delivery Modes {#event-modes}

#|
|| **Mode** | **Description** | **When to use** ||
|| `fetch` | The bot receives events via [imbot.v2.Event.get](./event-get.md) | For AI agents, server-side bots, and integrations without a persistent HTTP server ||
|| `webhook` | Bitrix24 sends events to the bot's URL via HTTP POST. The address is set in the required `fields.webhookUrl` field | For bots with a persistent HTTP server ||
|#

The mode is set in the `fields.eventMode` field when registering the bot with the [imbot.v2.Bot.register](../bots/bot-register.md) method, and changed with the [imbot.v2.Bot.update](../bots/bot-update.md) method.

Event data differs between the two modes:

- `fetch` — the event contains the full bot object, without the `auth` field
- `webhook` — the event contains a shortened bot object `{id, code}`. If the bot was registered by an application, the bot object contains the `auth` field with tokens

In `webhook` mode, all scalar values, such as numbers and `true`/`false`, come as strings, so the types have to be cast explicitly. For details, see [Bot Object Format](./events.md#bot-format).

## Event Subscription {#subscription}

Subscriptions to `ONIMBOTV2*` events are managed by the platform itself:

- it creates the subscription on [imbot.v2.Bot.register](../bots/bot-register.md) with `eventMode: "webhook"`
- it creates a subscription for the new address on [imbot.v2.Bot.update](../bots/bot-update.md) if a new `webhookUrl` is passed
- it removes the subscription on [imbot.v2.Bot.unregister](../bots/bot-unregister.md) or when the bot switches to `fetch` mode, if the application has no other bots left: subscriptions are shared by all bots of the application

An event of any bot is sent to the `webhookUrl` of all bots of this application. Identify the bot by the `data.bot.id` field. If the application has other bots, the previous address keeps receiving events even after `webhookUrl` is changed or the bot is removed.

Before switching a bot from `fetch` mode to `webhook`, collect the accumulated events: the `fetch` queue is deleted on the switch. Pass `webhookUrl` together with `eventMode`. Without it, a new subscription is not created. If the application has no other bots with a subscription, events stop arriving.

{% note warning "" %}

Do not call `event.bind` and `event.unbind` manually: otherwise, the subscriptions may diverge from those managed by Bitrix24.

{% endnote %}

## Limits {#limits}

#|
|| **Limit** | **Value** ||
|| Events per single [imbot.v2.Event.get](./event-get.md) call | 1–1000, 100 by default ||
|| Event retention period in `fetch` mode | 24 hours. Bitrix24 deletes older events even if the bot has not received them ||
|| Recommended polling interval in `fetch` mode | 5–30 seconds when there are no new events ||
|| Delivery in `webhook` mode | The platform expects HTTP 200. Retries on failure are not guaranteed ||
|#

For a polling cycle example in `fetch` mode, see [Polling Cycle](../../index.md#polling); for a handler example in `webhook` mode, see [Webhook Handler](../../index.md#webhook-handler).

## Server Availability for Sending and Receiving Events

In `webhook` mode, Bitrix24 sends events to the bot's address. How to verify that the request came from Bitrix24 is described in the [Webhook Notification Structure](./events.md#webhook-payload) section.

{% include notitle [Server Availability for Sending and Receiving Events](../../../../../_includes/events-index.md) %}

## Relationship with Other Objects {#relations}

Events connect the bot with chats: through them, the bot learns about messages and commands, and it responds with methods of other groups.

**Bot.** Events are delivered to a specific bot, so its `botId` is passed to [imbot.v2.Event.get](./event-get.md), and when calling via an inbound webhook, `botToken` as well. For details, see [Bots](../bots/index.md).

**Messages.** Most often, the bot processes the `ONIMBOTV2MESSAGE*` message events and the `ONIMBOTV2REACTIONCHANGE` reaction events. You can respond to them with the methods of the [Messages](../messages/index.md) group. Bots of type `supervisor` and `personal` can read a message by its ID with the [imbot.v2.Chat.Message.get](../messages/chat-message-get.md) method, and the neighboring messages with the [imbot.v2.Chat.Message.getContext](../messages/chat-message-get-context.md) method.

**Commands.** A slash command call arrives as the `ONIMBOTV2COMMANDADD` event. You can respond to the command with the [imbot.v2.Command.answer](../commands/command-answer.md) method. For details, see [Commands](../commands/index.md).

**Bot context.** If arbitrary data was passed to the bot when the chat was opened, for example about the task from which the chat was opened, it arrives in the `ONIMBOTV2CONTEXTGET` event. For details, see [Passing Context to a Bot](../bot-context.md).

**User events.** If you need to read the messenger event flow on behalf of a user rather than a bot, use the [im.v2.Event.*](../../im.v2/events/index.md) methods.

## Overview of Methods and Events {#all-methods}

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the methods: owner of the registered bot

### Methods

#|
|| **Method** | **Description** ||
|| [imbot.v2.Event.get](./event-get.md) | Returns bot events in `fetch` mode ||
|#

### Events

For the bot to work, it is enough to process `ONIMBOTV2MESSAGEADD`, `ONIMBOTV2COMMANDADD`, and `ONIMBOTV2JOINCHAT`, and in `webhook` mode also `ONIMBOTV2DELETE`. Process the remaining events if your bot needs them. The data of each event is described on the [Event Formats for imbot.v2](./events.md) page.

#|
|| **Event** | **Triggered** ||
|| [ONIMBOTV2MESSAGEADD](./events.md#onimbotv2messageadd) | When a message is sent to the bot in a one-on-one chat, with a bot mention, or as a reply to its message. Bots of type `personal` and `supervisor` receive all user messages in group chats they belong to, except messages from other bots ||
|| [ONIMBOTV2MESSAGEUPDATE](./events.md#onimbotv2messageupdate) | When a message is edited. Bitrix24 selects the recipients by the same rules as for `ONIMBOTV2MESSAGEADD` ||
|| [ONIMBOTV2MESSAGEDELETE](./events.md#onimbotv2messagedelete) | When a message is deleted. Only `messageId` comes instead of the `message` object: if you need the text of the deleted message, store it when you receive the message. Bitrix24 selects the recipients by the same rules as for `ONIMBOTV2MESSAGEADD` ||
|| [ONIMBOTV2REACTIONCHANGE](./events.md#onimbotv2reactionchange) | When a reaction to the bot's message is added or removed, if reactions are enabled for the bot ||
|| [ONIMBOTV2COMMANDADD](./events.md#onimbotv2commandadd) | When a user calls a slash command of the bot ||
|| [ONIMBOTV2JOINCHAT](./events.md#onimbotv2joinchat) | When the bot is added to a chat or invited to one ||
|| [ONIMBOTV2CONTEXTGET](./events.md#onimbotv2contextget) | When a dialog with the bot is opened, if context data is passed ||
|| [ONIMBOTV2DELETE](./events.md#onimbotv2delete) | When the bot is deleted from the system. The last event the bot receives in `webhook` mode. Not sent in `fetch` mode: the queue is deleted together with the bot ||
|#

## Continue Your Exploration

- [API Change Log for imbot.v2](../../change-log.md)
- [Events: Overview of Methods and Events](../../../../events/index.md)
- [Event Formats for imbot.v2](./events.md)
- [{#T}](../../index.md)
- [{#T}](../../entities.md)
- [{#T}](../../migration.md)
- [imbot.v2 Bots](../bots/index.md)
- [Commands imbot.v2](../commands/index.md)
- [{#T}](../bot-context.md)