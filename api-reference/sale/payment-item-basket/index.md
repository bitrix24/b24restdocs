# Binding Cart Items to Payments in the Online Store: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

An order can have multiple payments—for example, if products are paid for through different payment systems. A binding shows which cart items, and in what quantity, are included in each payment: it stores the payment ID `paymentId`, the cart item ID `basketId`, and the quantity `quantity`.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Payment Systems for Online Stores](https://helpdesk.bitrix24.com/open/25826751/)

## Relationship with Other Objects

A binding connects a payment and a cart item of the same order. The fields of the binding object are described in the reference [sale_payment_item_basket](../data-types.md#sale_payment_item_basket).

**Order.** The payment from `paymentId` and the cart item from `basketId` must belong to the same order. If the payment belongs to another order, the method [sale.paymentitembasket.add](./sale-payment-item-basket-add.md) returns the error `201240400002` — `payment not exists`.

**Payment.** One payment can include several cart items, and one cart item can be included in several payments. The pair “payment + cart item” is unique: binding the same pair again returns the error `201250000001`. When a payment is added with the method [sale.payment.add](../payment/sale-payment-add.md), Bitrix24 may create bindings for the cart items automatically — check them with the method [sale.paymentitembasket.list](./sale-payment-item-basket-list.md) before adding your own. The list of order payments is returned by the method [sale.payment.list](../payment/sale-payment-list.md).

**Cart.** The value of `quantity` cannot exceed the quantity of the cart item: for a larger value, the methods [sale.paymentitembasket.add](./sale-payment-item-basket-add.md) and [sale.paymentitembasket.update](./sale-payment-item-basket-update.md) return an error with the code `0` and the description “Insufficient item quantity in shopping cart”. The check is performed for each binding separately: Bitrix24 does not limit the total quantity across all payments. The quantity of a cart item is returned by the method [sale.basketitem.list](../basket-item/sale-basket-item-list.md) in the `quantity` field.

## How to Get Started

1. Retrieve the order ID using [sale.order.list](../order/sale-order-list.md).
2. Retrieve the order payments using [sale.payment.list](../payment/sale-payment-list.md) and the cart items using [sale.basketitem.list](../basket-item/sale-basket-item-list.md) — in both methods, filter by `orderId`.
3. Create a binding using [sale.paymentitembasket.add](./sale-payment-item-basket-add.md): pass `paymentId`, `basketId`, and `quantity`.
4. Check which cart items are bound to payments using [sale.paymentitembasket.list](./sale-payment-item-basket-list.md).
5. Pass the binding ID `id` from the response of the add or list method to the methods [sale.paymentitembasket.get](./sale-payment-item-basket-get.md), [sale.paymentitembasket.update](./sale-payment-item-basket-update.md), and [sale.paymentitembasket.delete](./sale-payment-item-basket-delete.md).

## Overview of Methods {#all-methods}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the methods: administrator

#|
|| **Method** | **Description** ||
|| [sale.paymentitembasket.add](./sale-payment-item-basket-add.md) | Adds a binding of a cart item to a payment ||
|| [sale.paymentitembasket.update](./sale-payment-item-basket-update.md) | Modifies the binding of a cart item to a payment ||
|| [sale.paymentitembasket.get](./sale-payment-item-basket-get.md) | Returns the values of all fields of the cart item binding to a payment ||
|| [sale.paymentitembasket.list](./sale-payment-item-basket-list.md) | Returns a list of bindings of cart items to payments ||
|| [sale.paymentitembasket.delete](./sale-payment-item-basket-delete.md) | Deletes the binding of a cart item to a payment ||
|| [sale.paymentitembasket.getfields](./sale-payment-item-basket-get-fields.md) | Returns the available fields of cart item bindings to payments ||
|#