# Working with Chat: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The `im.v2` methods allow you to receive user events while working with chats and manage files in chats without registering a separate chat bot.

{% note info "" %}

`im.v2` is a new generation of methods for working with chats. Currently, there are fewer of them than in the main section, but they will gradually replace the existing implementations.

The already existing messenger methods are available in the [Chats with User](../../../chats/index.md) section.

{% endnote %}

> Quick navigation: [all methods](#all-methods)

## Working with Events

User events in `im.v2` do not support webhook mode, unlike chat bot events. Events in this section are only available through polling: the application periodically retrieves the accumulated queue after subscribing to event records via the `im.v2.Event.*` methods.

1. Subscribe to event records via [im.v2.Event.subscribe](./events/event-subscribe.md).
2. Periodically retrieve new events via [im.v2.Event.get](./events/event-get.md).
3. Pass `offset` to confirm already processed events.
4. To stop recording, use [im.v2.Event.unsubscribe](./events/event-unsubscribe.md).

{% note info "What is polling mode" %}

Polling is a mode of receiving events where the application periodically requests accumulated events from the server. The server does not know the application's address and does not send anything on its own—it only accumulates the queue and delivers it upon request.

This distinguishes polling from webhook: in webhook mode, Bitrix24 calls the application's URL for each new event. Polling is convenient if the application does not have a permanent HTTP server or public URL.

{% endnote %}

## Working with Files

- Upload a file to the chat via [im.v2.File.upload](./files/file-upload.md).
- Get a download link for the uploaded file via [im.v2.File.download](./files/file-download.md).

## Overview of Methods {#all-methods}

> Scope: [`im`](../../../scopes/permissions.md)
>
> Who can perform methods: user or application with access to the messenger

### Events

#| 
|| **Method** | **Description** ||
|| [im.v2.Event.subscribe](./events/event-subscribe.md) | Subscribes to event records ||
|| [im.v2.Event.unsubscribe](./events/event-unsubscribe.md) | Stops event recording ||
|| [im.v2.Event.get](./events/event-get.md) | Returns events in polling mode ||
|| [im.v2 Events](./events/events.md) | Description of event formats and data structures ||
|#

### Files

#| 
|| **Method** | **Description** ||
|| [im.v2.File.upload](./files/file-upload.md) | Uploads a file to the chat ||
|| [im.v2.File.download](./files/file-download.md) | Returns a link to download the file ||
|#

## Continue Your Study

- [API imbot.v2 Change Log](../change-log.md)
- [im.v2 Events](./events/index.md)
- [im.v2 Files](./files/index.md)
- [{#T}](../index.md)