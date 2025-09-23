# Overview of Events When Working with Products

Events allow applications to respond to changes in almost real-time: receiving notifications about the creation, updating, or deletion of products, services, parent products, and variations.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to product events through the [application](./../../../../settings/app-installation/index.md) and the [event.bind](./../../../events/event-bind.md) method.

An example of a handler code for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../_includes/events-index.md) %}

## Overview of Events

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered** ||
|| [CATALOG.PRODUCT.ON.ADD](catalog-product-on-add.md)| When adding:

 - a product manually or via the [catalog.product.add](../catalog-product-add.md) method

 - a service manually or via the [catalog.product.service.add](../service/catalog-product-service-add.md) method

 - a parent product manually or via the [catalog.product.sku.add](../sku/catalog-product-sku-add.md) method

 - a variation manually or via the [catalog.product.offer.add](../offer/catalog-product-offer-add.md) method ||
|| [CATALOG.PRODUCT.ON.UPDATE](catalog-product-on-update.md)| When updating:

 - a product manually or via the [catalog.product.update](../catalog-product-update.md) method

 - a service manually or via the [catalog.product.service.update](../service/catalog-product-service-update.md) method

 - a parent product manually or via the [catalog.product.sku.update](../sku/catalog-product-sku-update.md) method

 - a variation manually or via the [catalog.product.offer.update](../offer/catalog-product-offer-update.md) method ||
|| [CATALOG.PRODUCT.ON.DELETE](catalog-product-on-delete.md)| When deleting:

 - a product manually or via the [catalog.product.delete](../catalog-product-delete.md) method

 - a service manually or via the [catalog.product.service.delete](../service/catalog-product-service-delete.md) method

 - a parent product manually or via the [catalog.product.sku.delete](../sku/catalog-product-sku-delete.md) method

 - a variation manually or via the [catalog.product.offer.delete](../offer/catalog-product-offer-delete.md) method ||
|#