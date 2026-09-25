# Deliveries in Payments: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A delivery item is a line in a CRM payment that shows which delivery the client pays for. For example, if the client pays for a product and courier delivery at once, the payment contains a product item and a delivery item.

The delivery itself is a separate document in the order of a deal or an invoice. It stores the delivery service, the cost, and the shipment status. The delivery item only refers to this document and takes the price from it. Items are managed with the [crm.item.payment.delivery.*](#all-methods) methods, and the delivery documents of a CRM object can be retrieved with the [crm.item.delivery.*](../../delivery/index.md) methods.

> Quick navigation: [all methods](#all-methods)

## Linking Deliveries in Payments with Other Objects

**CRM Payment.** The methods of this group work with the items of a specific payment. To add an item or retrieve the list of items, pass the payment's `paymentId`; to delete or relink an item, pass the item's `id`. When an item is added, relinked to another document, or deleted, Bitrix24 recalculates the payment amount based on the cost of its items.

Items can be changed only in an unpaid payment. If the client has already paid it, the modifying methods return the `ACCESS_DENIED` error — the same error as when permissions are insufficient.

**Delivery Document.** The [crm.item.payment.delivery.add](./crm-item-payment-delivery-add.md) method links an item to a document on creation, and [crm.item.payment.delivery.setDelivery](./crm-item-payment-delivery-set-delivery.md) relinks it to another document. The document is passed in the `deliveryId` parameter; the list of documents of a CRM object is returned by the [crm.item.delivery.list](../../delivery/crm-item-delivery-list.md) method.

**CRM Object.** A payment can be created with the [crm.item.payment.add](../crm-item-payment-add.md) method only for a deal or an invoice, so delivery items exist in payments of these objects.

## What a Delivery Item Consists Of

The delivery items of a payment are returned by the [crm.item.payment.delivery.list](./crm-item-payment-delivery-list.md) method. Each item has four fields:

#|
|| **Field** | **Description** ||
|| `id` | Item identifier. It is passed to the [crm.item.payment.delivery.setDelivery](./crm-item-payment-delivery-set-delivery.md) and [crm.item.payment.delivery.delete](./crm-item-payment-delivery-delete.md) methods ||
|| `paymentId` | Identifier of the payment that contains the item ||
|| `deliveryId` | Identifier of the delivery document — the `id` field from the [crm.item.delivery.list](../../delivery/crm-item-delivery-list.md) response ||
|| `quantity` | Quantity. For a delivery item, it is always `1` ||
|#

{% note warning "" %}

In the `deliveryId` parameter of the methods in this group, pass the `id` of the delivery document from the [crm.item.delivery.list](../../delivery/crm-item-delivery-list.md) response, not the `deliveryId` field from the same response: there, this name refers to the delivery service number. The document must belong to the same order as the payment. If the order has no such document, the [crm.item.payment.delivery.add](./crm-item-payment-delivery-add.md) method returns the `Shipment was not found` error.

{% endnote %}

## How to Work with Deliveries in Payment

1. Prepare the `paymentId` of an unpaid payment. You can retrieve it with the main payment methods [crm.item.payment.*](../index.md)
2. Retrieve the `id` of the delivery document with the [crm.item.delivery.list](../../delivery/crm-item-delivery-list.md) method
3. Add a delivery item using the method [crm.item.payment.delivery.add](./crm-item-payment-delivery-add.md)
4. Check the composition of items using the method [crm.item.payment.delivery.list](./crm-item-payment-delivery-list.md)
5. If necessary, relink the item to another document with the [crm.item.payment.delivery.setDelivery](./crm-item-payment-delivery-set-delivery.md) method or delete it with the [crm.item.payment.delivery.delete](./crm-item-payment-delivery-delete.md) method

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the methods: depending on the method

#| 
|| **Method** | **Description** ||
|| [crm.item.payment.delivery.add](./crm-item-payment-delivery-add.md) | Adds a delivery item to the payment ||
|| [crm.item.payment.delivery.list](./crm-item-payment-delivery-list.md) | Returns a list of delivery items for a specific payment ||
|| [crm.item.payment.delivery.delete](./crm-item-payment-delivery-delete.md) | Deletes a delivery item from the payment ||
|| [crm.item.payment.delivery.setDelivery](./crm-item-payment-delivery-set-delivery.md) | Reassigns the delivery item to another delivery document ||
|#

## Continue Learning

- [{#T}](../index.md)
- [{#T}](../../delivery/index.md)
- [{#T}](../products-in-payment/index.md)