# Overview of Events When Working with Price Types

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Events allow applications to respond to changes almost in real-time: receiving notifications about the creation, updating, or deletion of a price type. The methods for working with price types are described in the article [{#T}](../index.md).

Detailed information on working with events is described in the article [Concept and Benefits of Event Handling](../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to price type events through the [application](../../../../settings/app-installation/index.md) and the [event.bind](../../../events/event-bind.md) method.

An example of a handler code for the event is described in the article [How to Test Your Handler for Handling Bitrix24 Events](../../../events/test-handler.md).

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../settings/app-installation/installation-finish.md)

## What the Handler Receives

All three events pass only the price type identifier in the `data.FIELDS` object:

```json
{
    "event": "CATALOG.PRICE.TYPE.ON.UPDATE",
    "event_handler_id": 1,
    "data": {
        "FIELDS": {
            "ID": 1
        }
    },
    "ts": 1714649632
}
```

When a price type is added, two events arrive: first `CATALOG.PRICE.TYPE.ON.ADD`, then `CATALOG.PRICE.TYPE.ON.UPDATE` with the same `ID`. The events are triggered by the methods of both API versions: `catalog.priceType.*` and `catalog.pricetype.*` from [REST 3.0](../../../rest-v3.md).

The event contains no field values and no list of changes. To find out what changed, request the price type using the [catalog.priceType.get](../catalog-price-type-get.md) method and compare the data with your copy. After the `CATALOG.PRICE.TYPE.ON.DELETE` event, this method returns the `201000000000` error because the price type no longer exists.

The request also contains the `auth` key. It holds the authorization parameters and account data. Its structure is described on the event page, for example [CATALOG.PRICE.TYPE.ON.ADD](./catalog-price-type-on-add.md#auth).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered** ||
|| [CATALOG.PRICE.TYPE.ON.ADD](./catalog-price-type-on-add.md) | When a price type is added manually or using [catalog.priceType.add](../catalog-price-type-add.md), [catalog.pricetype.add](../catalog-price-type-add-rest-v3.md) ||
|| [CATALOG.PRICE.TYPE.ON.UPDATE](./catalog-price-type-on-update.md) | When a price type is updated manually or using [catalog.priceType.update](../catalog-price-type-update.md), [catalog.pricetype.update](../catalog-price-type-update-rest-v3.md), and also when a price type is added ||
|| [CATALOG.PRICE.TYPE.ON.DELETE](./catalog-price-type-on-delete.md) | When a price type is deleted manually or using [catalog.priceType.delete](../catalog-price-type-delete.md), [catalog.pricetype.delete](../catalog-price-type-delete-rest-v3.md) ||
|#
