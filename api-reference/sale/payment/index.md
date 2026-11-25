# Payments in the Online Store: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Payments contain information about payments for orders: payment system, status and date of payment, payer identifier.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Payment Systems for Online Stores](https://helpdesk.bitrix24.com/open/25826751/)
> 
> User documentation: [Track the payment status in the deal form](https://helpdesk.bitrix24.com/open/18367058/)

## Connection of Payments with Other Objects

**Order.** Specify the order for which you are creating the payment. You can get a list of orders using the [sale.order.list](../order/sale-order-list.md) method.

**Payment Systems.** Specify the payment system. You can get a list of payment systems using the [sale.paysystem.list](../../pay-system/sale-pay-system-list.md) method.

**Linking Cart Items to Payment.** Select the cart items for which you want to create a payment. Use the [sale.paymentitembasket.*](../payment-item-basket/index.md) methods.

**Linking Payments to Shipments.** Specify which shipments the payment is linked to. Use the [sale.paymentitemshipment.*](../payment-item-shipment/index.md) methods.

## How to Get Started

1. Create an order using [sale.order.add](../order/sale-order-add.md) or find an existing order using [sale.order.get](../order/sale-order-get.md) or [sale.order.list](../order/sale-order-list.md).
2. Create a payment using [sale.payment.add](./sale-payment-add.md) and specify the order ID.
3. Link the payment to cart items using [sale.paymentitembasket.*](../payment-item-basket/index.md) or to shipments using [sale.paymentitemshipment.*](../payment-item-shipment/index.md).
4. Check payment data using [sale.payment.get](./sale-payment-get.md) or [sale.payment.list](./sale-payment-list.md).

The `sale.payment.add`, `sale.payment.get`, and `sale.payment.update` methods return the payment in the `payment` object. The `sale.payment.list` method returns an array of payments in the `payments` field and the total number of records found in the `total` field.

Date and time fields are returned in ISO 8601 format in the Bitrix24 server time zone, for example `2024-04-10T10:00:00+02:00`. The user's time zone does not affect the value. A value without an offset in a request is treated as server time. A value with an offset is converted to the server time zone: `2024-04-10T10:00:00+03:00` is returned as `2024-04-10T09:00:00+02:00`.

The `date` field `payReturnDate` is also returned with a time and offset: the date `2024-04-10` is returned as `2024-04-10T02:00:00+02:00`. Use only the date from this value.

## Overview of Methods {#all-methods}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the methods: retrieve payments — store manager, retrieve the field description — any user, add, update, and delete payments — administrator

#| 
|| **Method** | **Description** ||
|| [sale.payment.add](./sale-payment-add.md) | Adds a payment ||
|| [sale.payment.update](./sale-payment-update.md) | Modifies a payment ||
|| [sale.payment.get](./sale-payment-get.md) | Returns information about a payment ||
|| [sale.payment.list](./sale-payment-list.md) | Returns a list of payments ||
|| [sale.payment.delete](./sale-payment-delete.md) | Deletes a payment ||
|| [sale.payment.getFields](./sale-payment-get-fields.md) | Returns available payment fields ||
|#
