# Events: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

This section describes the methods for subscribing to user events from the messenger and retrieving them in polling mode. Use these methods if the integration needs to read the stream of events on behalf of a user or application.

> Quick navigation: [all methods](#all-methods)

## How to Work with Events

1. Enable event recording via [im.v2.Event.subscribe](./event-subscribe.md).
2. Retrieve accumulated events through [im.v2.Event.get](./event-get.md).
3. Pass `offset` to confirm already processed records.
4. Terminate the subscription using the [im.v2.Event.unsubscribe](./event-unsubscribe.md) method.

{% note info "What is polling mode" %}

Polling is a mode of receiving events where the application periodically requests accumulated events from the server. The server does not know the application's address and does not send anything on its own—it only accumulates a queue and delivers it upon request.

This distinguishes polling from webhook: in webhook mode, Bitrix24 calls the application's URL for each new event. Polling is convenient if the application does not have a permanent HTTP server or a public URL.

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`im`](../../../../scopes/permissions.md)
>
> Who can execute the methods: a user or an application with access to the messenger

#| 
|| **Method** | **Description** ||
|| [im.v2.Event.subscribe](./event-subscribe.md) | Subscribes to event recording ||
|| [im.v2.Event.unsubscribe](./event-unsubscribe.md) | Stops event recording ||
|| [im.v2.Event.get](./event-get.md) | Returns events in polling mode ||
|| [Event Formats](./events.md) | Description of events and data structures ||
|#

## Continue Your Exploration

- [Change Log for API imbot.v2](../../change-log.md)
- [im.v2: Overview of Methods](../index.md)
- [Event Formats for im.v2](./events.md)