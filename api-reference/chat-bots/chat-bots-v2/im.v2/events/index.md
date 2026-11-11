# Overview of im.v2 Methods and Events

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

With the `im.v2.Event.*` methods, an integration learns about events in the user's chats: new, edited, and deleted messages, reactions, and new participants. To do this, it subscribes the user to events and collects them itself with requests — this is polling mode. For example, an AI agent can monitor new messages in an employee's chats this way and reply to them.

> Quick navigation: [all methods and events](#all-methods)

## How to Work with Events {#how-to-start}

1. Enable event recording with the [im.v2.Event.subscribe](./event-subscribe.md) method. Bitrix24 starts queuing events for the user on whose behalf the method is called. A repeated call does not reset the accumulated events.
2. Retrieve accumulated events through [im.v2.Event.get](./event-get.md).
3. Confirm the processed events: pass the `result.nextOffset` value from the response in `offset` of the next call, and the method removes them from the queue. If the events could not be processed, repeat the call with the same `offset` — the same events are returned again.
4. When you no longer need events, cancel the subscription with the [im.v2.Event.unsubscribe](./event-unsubscribe.md) method. The accumulated events are deleted.

{% note info "What is polling mode" %}

Polling is a mode in which the application itself periodically requests the accumulated events. Bitrix24 does not know the application's address and does not send anything on its own: it accumulates events in a queue and delivers them on request.

Webhook mode works the other way around: Bitrix24 itself calls the application's URL with each new event. Polling is convenient if the application does not have a permanent HTTP server or a public URL.

{% endnote %}

## Limits and Restrictions {#limits}

#|
|| **Limit** | **Value** ||
|| Events per single `im.v2.Event.get` call | 1–1000, 100 by default ||
|| Event order | Ascending by `eventId` ||
|| Storage period of recorded events | 24 hours ||
|| Delivery method | Polling only. Webhook mode is not supported for user events ||
|| Number of recipients | A user's event queue is designed for a single application. If several applications subscribe the same user, a call to `im.v2.Event.get` with `offset` from any of them deletes the events for all of them ||
|#

{% note warning "" %}

Do not subscribe the same user from several applications: they will take events away from each other. This is intentional: a single agent reacts to a user's events immediately, for example an AI assistant that replies to messages. If several agents process the same events, their replies are duplicated and contradict each other. If you need several independent handlers, subscribe different users to events, one per handler. Or register a separate bot for each handler: each bot has its own event queue.

{% endnote %}

## Common Mistakes When Working with the Queue {#pitfalls}

The `im.v2.Event.*` methods return almost no errors. If an integration works with the queue incorrectly, the call usually succeeds, but events do not arrive or get lost.

#|
|| **Situation** | **What happens** | **What to do** ||
|| The user is not subscribed | [im.v2.Event.get](./event-get.md) returns an empty `events` array without an error | Call [im.v2.Event.subscribe](./event-subscribe.md). Events that occurred before the subscription will not appear ||
|| [im.v2.Event.unsubscribe](./event-unsubscribe.md) was called | The accumulated events are deleted. After a new subscription, only new events arrive | Collect unprocessed events before unsubscribing ||
|| A value greater than `nextOffset` was passed in `offset` | The method deletes events with IDs lower than `offset`, including those not yet received | Pass only `nextOffset` from the previous response in `offset` ||
|| A non-numeric value was passed in `offset` or `limit` | Error `100` with HTTP status 400 | Pass integers ||
|#

## How This Differs from Chatbot Events {#vs-imbot}

#|
|| **If You Need To** | **Open the Section** ||
|| Read the event stream on behalf of a user or application, without registering a bot | The `im.v2.Event.*` methods on this page ||
|| Receive bot events — incoming messages, command calls, adding the bot to a chat | [Events imbot.v2](../../imbot.v2/events/index.md) ||
|#

Two more differences:

- `im.v2` events must be enabled with a subscription, while the bot receives `ONIMBOTV2*` events without a separate subscription
- `im.v2` events are available only through polling, while bot events can also be received in webhook mode

## Overview of Methods and Events {#all-methods}

> Scope: [`im`](../../../../scopes/permissions.md)
>
> Who can execute methods: user or application with access to the messenger

### Methods

#| 
|| **Method** | **Description** ||
|| [im.v2.Event.subscribe](./event-subscribe.md) | Enables recording of the user's events ||
|| [im.v2.Event.unsubscribe](./event-unsubscribe.md) | Stops event recording and deletes the accumulated events ||
|| [im.v2.Event.get](./event-get.md) | Returns events in polling mode ||
|#

### Events {#event-types}

The event type comes in the `result.events[].type` field of the [im.v2.Event.get](./event-get.md) response, and the data in the `result.events[].data` field. The data of each event is described on the [Event Formats for im.v2](./events.md) page.

#|
|| **Event** | **Triggered** ||
|| [ONIMV2MESSAGEADD](./events.md#onimv2messageadd) | When a new message appears in a chat the subscribed user belongs to ||
|| [ONIMV2MESSAGEUPDATE](./events.md#onimv2messageupdate) | When a message is edited in a chat. If the message is updated with the `im.message.update` method, the event is not sent ||
|| [ONIMV2MESSAGEDELETE](./events.md#onimv2messagedelete) | When a message is deleted in a chat. Only `messageId` comes instead of the `message` object ||
|| [ONIMV2REACTIONCHANGE](./events.md#onimv2reactionchange) | When a reaction to a message in a chat is added or removed. The `action` field contains an empty string, so it cannot distinguish between adding and removing a reaction ||
|| [ONIMV2JOINCHAT](./events.md#onimv2joinchat) | When a new participant is added to a chat ||
|#

## Continue Your Exploration

- [Change Log for API imbot.v2](../../change-log.md)
- [im.v2: Overview of Methods](../index.md)
- [Events: Overview of Methods and Events](../../../../events/index.md)
- [Event Formats for im.v2](./events.md)
- [Files im.v2](../files/index.md)
- [Events imbot.v2](../../imbot.v2/events/index.md)