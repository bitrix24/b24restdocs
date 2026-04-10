# Overview of Events When Working with the Connector

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Events allow applications to respond to changes in open line connectors: receiving notifications about new messages, their modifications, deletions, as well as the completion of dialogues and disconnection of lines.

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
|| [OnImConnectorMessageAdd](on-im-connector-message-add.md) | When messages are sent manually ||
|| [OnImConnectorDialogStart](on-im-connector-dialog-start.md) | When a dialogue is created manually ||
|| [OnImConnectorMessageUpdate](on-im-connector-message-update.md) | When a message is modified manually ||
|| [OnImConnectorMessageDelete](on-im-connector-message-delete.md) | When a message is deleted manually ||
|| [OnImConnectorDialogFinish](on-im-connector-dialog-finish.md) | When a dialogue is closed manually ||
|| [OnImConnectorStatusDelete](on-im-connector-status-delete.md) | When an open line is disconnected manually ||
|| [OnImConnectorLineDelete](on-im-connector-line-delete.md) | When an open line is deleted manually ||
|#