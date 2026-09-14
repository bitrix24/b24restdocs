# Overview of Events When Working with Open Channels

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Events allow applications to respond to changes almost in real-time: receiving notifications about the creation, updating, or deletion of messages in the open channel chat.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to open channel events through the [application](../../../../settings/app-installation/index.md) and the [event.bind](../../../events/event-bind.md) method.

An example of a handler for the event is described in the article [How to Test Your Handler for Processing Events in Bitrix24](../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server availability](../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered** ||
|| [OnSessionStart](./on-session-start.md) | When creating an Open Channel session manually or using the [imopenlines.session.start](../sessions/imopenlines-session-start.md) method ||
|| [OnOpenLineMessageAdd](./on-open-line-message-add.md) | When a message is added to the chat manually or by the method [imopenlines.crm.message.add](../messages/imopenlines-crm-message-add.md) ||
|| [OnOpenLineMessageUpdate](./on-open-line-message-update.md) | When a message in the chat is modified manually ||
|| [OnOpenLineMessageDelete](./on-open-line-message-delete.md) | When a message in the chat is deleted manually ||
|| [OnSessionFinish](./on-session-finish.md) | When closing an Open Channel session manually ||
|#
