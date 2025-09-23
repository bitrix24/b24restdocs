# Overview of Events When Working with Prices

Events allow applications to respond to changes almost in real-time: receiving notifications about the creation, update, or deletion of a price.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to price events through the [application](./../../../../settings/app-installation/index.md) and the [event.bind](./../../../events/event-bind.md) method.

An example of a handler code for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../_includes/events-index.md) %}

## Overview of Events

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered** ||
|| [CATALOG.PRICE.ON.ADD](catalog-price-on-add.md)| When a price is added manually or via the [catalog.price.add](../catalog-price-add.md) method ||
|| [CATALOG.PRICE.ON.UPDATE](catalog-price-on-update.md)| When a price is updated manually or via the [catalog.price.update](../catalog-price-update.md) method ||
|| [CATALOG.PRICE.ON.DELETE](catalog-price-on-delete.md)| When a price is deleted manually or via the [catalog.price.delete](../catalog-price-delete.md) method ||
|#