# Overview of Events When Working with Price Types

Events allow applications to respond to changes almost in real-time: receiving notifications about the creation, updating, or deletion of a price type.

Detailed information on working with events is described in the article [Concept and Benefits of Event Handling](../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to price type events through the [application](./../../../../settings/app-installation/index.md) and the [event.bind](./../../../events/event-bind.md) method.

An example of a handler code for the event is described in the article [How to Test Your Handler for Handling Bitrix24 Events](../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../_includes/events-index.md) %}

## Overview of Events

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered** ||
|| [CATALOG.PRICE.TYPE.ON.ADD](catalog-price-type-on-add.md)| When a price type is added manually or via the [catalog.priceType.add](../catalog-price-type-add.html) method ||
|| [CATALOG.PRICE.TYPE.ON.UPDATE](catalog-price-type-on-update.md)| When a price type is updated manually or via the [catalog.priceType.update](../catalog-price-type-update.html) method ||
|| [CATALOG.PRICE.TYPE.ON.DELETE](catalog-price-type-on-delete.md)| When a price type is deleted manually or via the [catalog.priceType.delete](../catalog-price-type-delete.html) method ||
|#