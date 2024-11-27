# Order in the Online Store: Overview of Methods

An order is a record of a purchase. An order contains customer data, a list of products or services, their quantity, cost, and the status of the order.

> Quick navigation: [all methods](#all-methods)
> 
> User documentation: [How to create an order within CRM](https://helpdesk.bitrix24.com/open/8271153/)

## Products and Services in the Order

When a customer places an order, products and services are first added to the cart, and then the cart is linked to the order. It is not possible to add a product directly to the order, bypassing the cart stage.

## Connection of the Order with Other Objects

**Types of Payers.** Determine what type of client the buyer is: individual or legal entity. For this, use the methods [sale.persontype.*](../person-type/index.md).

**Currency.** Choose the currency in which the order will be paid. The list of currencies can be obtained using the method [crm.currency.list](../../crm/currency/crm-currency-list.md).

**Order Properties.** Create fields that the buyer must fill out when placing an order, such as "Metro Station" or "Date and Time of Delivery." To create order properties, use the methods [sale.property.*](../property/index.md).

**Status.** Track the progress of order fulfillment using statuses. You can create and change a status using the group of methods [sale.status.*](../status/index.md).

**Cart.** Add or modify products in the cart of an existing order using the methods [sale.basketitem.*](../basket-item/index.md).

**Payments.** Create and modify order payments using the methods [sale.payment.*](../payment/index.md).

**Shipments.** Control the shipment of products to customers using the methods [sale.shipment.*](../shipment/index.md).

## Overview of Methods {#all-methods}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can perform the methods: administrator

#|
|| **Method** | **Description** ||
|| [sale.order.add](./sale-order-add.md) | Adds an order ||
|| [sale.order.update](./sale-order-update.md) | Modifies an order ||
|| [sale.order.get](./sale-order-get.md) | Returns order fields and fields of related objects ||
|| [sale.order.list](./sale-order-list.md) | Returns a list of orders ||
|| [sale.order.delete](./sale-order-delete.md) | Deletes an order and related objects ||
|| [sale.order.getFields](./sale-order-get-fields.md) | Returns order fields ||
|#