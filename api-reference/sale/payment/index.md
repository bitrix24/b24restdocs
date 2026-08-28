# Payments in the Online Store: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

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

**Linking Payments to Shipments.** Specify which shipments have been paid. Use the [sale.paymentItemShipment.*](../payment-item-shipment/index.md) methods.

## How to Get Started

1. Create an order using [sale.order.add](../order/sale-order-add.md) or find an existing order using [sale.order.get](../order/sale-order-get.md) or [sale.order.list](../order/sale-order-list.md).
2. Create a payment using [sale.payment.add](./sale-payment-add.md) and specify the order ID.
3. Link the payment to cart items using [sale.paymentitembasket.*](../payment-item-basket/index.md) or to shipments using [sale.paymentItemShipment.*](../payment-item-shipment/index.md).
4. Check payment data using [sale.payment.get](./sale-payment-get.md) or [sale.payment.list](./sale-payment-list.md).

## Overview of Methods {#all-methods}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the methods: administrator

#| 
|| **Method** | **Description** ||
|| [sale.payment.add](./sale-payment-add.md) | Adds a payment ||
|| [sale.payment.update](./sale-payment-update.md) | Modifies a payment ||
|| [sale.payment.get](./sale-payment-get.md) | Returns information about a payment ||
|| [sale.payment.list](./sale-payment-list.md) | Returns a list of payments ||
|| [sale.payment.delete](./sale-payment-delete.md) | Deletes a payment ||
|| [sale.payment.getFields](./sale-payment-get-fields.md) | Returns available payment fields ||
|#