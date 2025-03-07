# Binding Cart Items to Payments in the Online Store: Overview of Methods

A single cart can have multiple payments—for example, if products are paid for through different payment systems. Determine which cart items correspond to each payment.

> Quick navigation: [all methods](#all-methods)

## Relationship of Cart Item Binding to Payments with Other Objects

**Payments.** Specify the payment ID to which you want to bind the cart item. A list of payment IDs can be obtained using the [sale.payment.list](../payment/sale-payment-list.md) method.

**Cart.** Specify the cart item ID for the payment. A list of cart item IDs can be obtained using the [sale.basketitem.list](../basket-item/sale-basket-item-list.md) method.

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