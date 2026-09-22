# Linking Payments to Shipments in the Online Store: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

An order can include several independent shipments. To specify which shipments a payment relates to, use payment-to-shipment links.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Payment Systems for Online Stores](https://helpdesk.bitrix24.com/open/25826751/)

## How to Get Started

1. Retrieve the payment ID using [sale.payment.list](../payment/sale-payment-list.md).
2. Retrieve the shipment ID using [sale.shipment.list](../shipment/sale-shipment-list.md).
3. Check the available link fields using [sale.paymentItemShipment.getFields](./sale-payment-item-shipment-get-fields.md).
4. Create a link using [sale.paymentItemShipment.add](./sale-payment-item-shipment-add.md).
5. Use [sale.paymentItemShipment.list](./sale-payment-item-shipment-list.md) to verify the relationships between payments and shipments.

## How the Link Works

A link stores the payment identifier `paymentId`, shipment identifier `shipmentId`, and optional external identifier `xmlId`. It has no amount or quantity fields: it identifies the relationship between a payment and a shipment but does not store an amount distributed between them.

To link one payment to two shipments, create two links with the same `paymentId` and different `shipmentId` values:

```json
[
    { "fields": { "paymentId": 1025, "shipmentId": 2471 } },
    { "fields": { "paymentId": 1025, "shipmentId": 2472 } }
]
```

Pass each item from the example in a separate [sale.paymentItemShipment.add](./sale-payment-item-shipment-add.md) call, not as a single array. Retrieve payment identifiers using [sale.payment.list](../payment/sale-payment-list.md) and shipment identifiers using [sale.shipment.list](../shipment/sale-shipment-list.md). The same `paymentId` + `shipmentId` pair can exist only once.

## Overview of Methods {#all-methods}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the methods: administrator

#|
|| **Method** | **Description** ||
|| [sale.paymentItemShipment.add](./sale-payment-item-shipment-add.md) | Adds a payment link to a shipment ||
|| [sale.paymentItemShipment.update](./sale-payment-item-shipment-update.md) | Modifies the payment link to a shipment ||
|| [sale.paymentItemShipment.get](./sale-payment-item-shipment-get.md) | Returns the values of the payment link fields for a shipment by its ID ||
|| [sale.paymentItemShipment.list](./sale-payment-item-shipment-list.md) | Returns a list of payment links to shipments based on a filter ||
|| [sale.paymentItemShipment.delete](./sale-payment-item-shipment-delete.md) | Deletes the payment link to a shipment ||
|| [sale.paymentItemShipment.getFields](./sale-payment-item-shipment-get-fields.md) | Returns the fields of the payment link to a shipment ||
|#
