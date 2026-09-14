# Overview of Events When Working with Open Channel Connectors

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Open Channel connector events notify the application about external channel messages, the start and completion of dialogues, channel status disconnection, and line deletion.

Detailed work with events is described in the article [Concept and Benefits of Event Handling](../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to connector events through the [application](../../../../settings/app-installation/index.md) and the [event.bind](../../../events/event-bind.md) method.

An example of a handler code for the event is described in the article [How to Test Your Handler for Handling Bitrix24 Events](../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`imconnector`](../../../scopes/permissions.md), [`imopenlines`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered** ||
|| [OnImConnectorMessageAdd](on-im-connector-message-add.md) | When a message is sent from Bitrix24 to an external channel ||
|| [OnImConnectorDialogStart](on-im-connector-dialog-start.md) | When a dialogue is created in an external channel ||
|| [OnImConnectorMessageUpdate](on-im-connector-message-update.md) | When a message is modified in an external channel ||
|| [OnImConnectorMessageDelete](on-im-connector-message-delete.md) | When a message is deleted in an external channel ||
|| [OnImConnectorDialogFinish](on-im-connector-dialog-finish.md) | When a dialogue is closed in an external channel ||
|| [OnImConnectorStatusDelete](on-im-connector-status-delete.md) | When a channel status is disconnected ||
|| [OnImConnectorLineDelete](on-im-connector-line-delete.md) | When an open line is deleted ||
|#
