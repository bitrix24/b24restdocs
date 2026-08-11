# Events: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The methods subscribe the user to messenger events and return them in polling mode. Use them if the integration needs to read the stream of events on behalf of the user or application.

> Quick navigation: [all methods and events](#all-methods)

## How to Work with Events {#how-to-start}

1. Enable event recording via [im.v2.Event.subscribe](./event-subscribe.md). The method is idempotent: a repeated call is safe.
2. Retrieve accumulated events through [im.v2.Event.get](./event-get.md).
3. Pass `offset` to confirm already processed records. Take the value from the `nextOffset` field of the previous response.
4. Terminate the subscription using the [im.v2.Event.unsubscribe](./event-unsubscribe.md) method.

{% note info "What is polling mode" %}

Polling is a mode of receiving events where the application periodically requests accumulated events from the server. The server does not know the application's address and does not send anything on its own—it only accumulates a queue and delivers it upon request.

This distinguishes polling from webhook: in webhook mode, Bitrix24 calls the application's URL with each new event. Polling is convenient if the application does not have a permanent HTTP server or public URL.

{% endnote %}

Without a subscription, events are not recorded, and [im.v2.Event.get](./event-get.md) returns an empty `events` array. After unsubscribing, the already accumulated events remain available until the storage period expires or until they are confirmed with `offset`.

## Limits and Restrictions {#limits}

#|
|| **Limit** | **Value** ||
|| Events per single `im.v2.Event.get` call | 1–1000, 100 by default ||
|| Storage period of recorded events | 24 hours ||
|| Delivery method | Polling only. Webhook mode is not supported for user events ||
|| Number of recipients | The events of a single user can be received by only one application. If several applications subscribe the same user, a call with `offset` confirms and removes the records for all of them ||
|#

{% note warning "" %}

The single-recipient restriction is intentional: the assumption is that one agent reacts to a user's events immediately. If you need several independent handlers, use different user contexts or webhook subscriptions.

{% endnote %}

## How This Differs from Chatbot Events {#vs-imbot}

#|
|| **If You Need To** | **Open the Section** ||
|| Read the event stream on behalf of a user or application, without registering a bot | The `im.v2.Event.*` methods on this page ||
|| Receive bot events — incoming messages, command calls, adding the bot to a chat | [Events imbot.v2](../../imbot.v2/events/index.md) ||
|#

There are two differences. The `im.v2` events require an explicit subscription, whereas the subscription to the `ONIMBOTV2*` bot events is created automatically when the bot is registered. In addition, the `im.v2` events are available only through polling, while bot events can also be received in webhook mode.

## Overview of Methods and Events {#all-methods}

> Scope: [`im`](../../../../scopes/permissions.md)
>
> Who can execute methods: user or application with access to the messenger

### Methods

#| 
|| **Method** | **Description** ||
|| [im.v2.Event.subscribe](./event-subscribe.md) | Subscribes to event recording ||
|| [im.v2.Event.unsubscribe](./event-unsubscribe.md) | Stops event recording ||
|| [im.v2.Event.get](./event-get.md) | Returns events in polling mode ||
|| [Event Formats](./events.md) | Description of events and data structures ||
|#

### Events {#event-types}

The event type arrives in the `events[].type` field of the [im.v2.Event.get](./event-get.md) response, and the data — in the `events[].data` field.

#|
|| **Event** | **Triggered** ||
|| [ONIMV2MESSAGEADD](./events.md#onimv2messageadd) | When a new message appears in a chat the subscribed user belongs to ||
|| [ONIMV2MESSAGEUPDATE](./events.md#onimv2messageupdate) | When a message is edited in a chat ||
|| [ONIMV2MESSAGEDELETE](./events.md#onimv2messagedelete) | When a message is deleted in a chat ||
|| [ONIMV2REACTIONCHANGE](./events.md#onimv2reactionchange) | When a reaction to a message in a chat is added or removed ||
|| [ONIMV2JOINCHAT](./events.md#onimv2joinchat) | When a new participant is added to a chat ||
|#

## Continue Your Exploration

- [Change Log for API imbot.v2](../../change-log.md)
- [im.v2: Overview of Methods](../index.md)
- [Event Formats for im.v2](./events.md)
- [Files im.v2](../files/index.md)
- [Events imbot.v2](../../imbot.v2/events/index.md)