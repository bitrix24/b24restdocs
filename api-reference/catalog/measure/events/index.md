# Measurement Unit Events: Overview of Events

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Events allow applications to respond to changes in almost real-time: receiving notifications about the creation, update, or deletion of a measurement unit.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

> Quick navigation: [all events](#all-events)
>
> User documentation: [How to add and configure units of measurement in CRM](https://helpdesk.bitrix24.com/open/7921829/)

## How to Receive Events

You can subscribe to measurement unit events through the [application](../../../../settings/app-installation/index.md) and the [event.bind](../../../events/event-bind.md) method.

An example of a handler code for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered** ||
|| [CATALOG.MEASURE.ON.ADD](./catalog-measure-on-add.md) | When a measurement unit is added manually or using [catalog.measure.add](../catalog-measure-add.md) ||
|| [CATALOG.MEASURE.ON.UPDATE](./catalog-measure-on-update.md) | When a measurement unit is updated manually or using [catalog.measure.update](../catalog-measure-update.md) ||
|| [CATALOG.MEASURE.ON.DELETE](./catalog-measure-on-delete.md) | When a measurement unit is deleted manually or using [catalog.measure.delete](../catalog-measure-delete.md) ||
|#
