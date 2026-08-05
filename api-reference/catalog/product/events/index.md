# Product Events: Overview of Events

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Events allow applications to respond to changes in almost real-time: receiving notifications about the creation, updating, or deletion of products, services, parent products, and variations.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

> Quick navigation: [all events](#all-events)
>
> User documentation: [Create and configure product catalog](https://helpdesk.bitrix24.com/open/21121728/)

## How to Receive Events

You can subscribe to product events through the [application](../../../../settings/app-installation/index.md) and the [event.bind](../../../events/event-bind.md) method.

An example of a handler code for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered** ||
|| [CATALOG.PRODUCT.ON.ADD](./catalog-product-on-add.md) | When a product, service, parent product, or variation is added manually or using [catalog.product.add](../catalog-product-add.md), [catalog.product.service.add](../service/catalog-product-service-add.md), [catalog.product.sku.add](../sku/catalog-product-sku-add.md), [catalog.product.offer.add](../offer/catalog-product-offer-add.md) ||
|| [CATALOG.PRODUCT.ON.UPDATE](./catalog-product-on-update.md) | When a product, service, parent product, or variation is updated manually or using [catalog.product.update](../catalog-product-update.md), [catalog.product.service.update](../service/catalog-product-service-update.md), [catalog.product.sku.update](../sku/catalog-product-sku-update.md), [catalog.product.offer.update](../offer/catalog-product-offer-update.md) ||
|| [CATALOG.PRODUCT.ON.DELETE](./catalog-product-on-delete.md) | When a product, service, parent product, or variation is deleted manually or using [catalog.product.delete](../catalog-product-delete.md), [catalog.product.service.delete](../service/catalog-product-service-delete.md), [catalog.product.sku.delete](../sku/catalog-product-sku-delete.md), [catalog.product.offer.delete](../offer/catalog-product-offer-delete.md) ||
|#
