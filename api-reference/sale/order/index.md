# Order in the Online Store: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

An order is a record of a purchase: the customer, products or services, their quantity, cost, and fulfillment status. The `sale.order.*` methods create, update, retrieve, and delete Online Store orders.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Place an Order on the Website](https://helpdesk.bitrix24.com/open/17300484/)
> 
> User documentation: [How to create an order within CRM](https://helpdesk.bitrix24.com/open/8271153/)

## How to Get Started with an Order

1. Retrieve the payer type using [sale.persontype.list](../person-type/sale-person-type-list.md)
2. Create an order using [sale.order.add](./sale-order-add.md): pass the site ID `lid`, for example `s1`, the payer type `personTypeId`, and the currency `currency`. Note the order `id` returned in the response
3. Add products or services to the order using [sale.basketitem.*](../basket-item/index.md), passing this `id` in the `orderId` field. A product cannot be added to an order without going through the cart
4. Create payments using [sale.payment.*](../payment/index.md) and shipments using [sale.shipment.*](../shipment/index.md)
5. Check the order using [sale.order.get](./sale-order-get.md): it returns the order together with its basket items, payments, and shipments
6. Change the order status using [sale.order.update](./sale-order-update.md): pass the new `statusId`

## What to Consider

- The `lid`, `personTypeId`, `currency`, and `userId` fields can only be set when an order is created. The [sale.order.update](./sale-order-update.md) method does not change them and does not return an error
- The [sale.order.list](./sale-order-list.md) method returns orders in the `result.orders` array, up to 50 per call. The add, update, and get methods return a single order in `result.order`
- The [sale.order.list](./sale-order-list.md) method silently skips a `filter` condition with an unknown field name. The exact field names are returned by [sale.order.getFields](./sale-order-get-fields.md)
- An order that has a payment with `paid` = `Y` cannot be deleted: [sale.order.delete](./sale-order-delete.md) returns the `SALE_ORDER_CANCEL_PAYMENT_EXIST_ACTIVE` error

Error codes are listed on the method pages, and the common REST errors are described in the [Error Codes](../../../error-codes.md) article.

## Connection of the Order with Other Objects

An order references the payer type, currency, and status through its fields and stores property values, while the cart, payments, and shipments reference the order.

**Payer Type.** The `personTypeId` field determines what type of client the buyer is: individual or legal entity. Payer types are configured by the [sale.persontype.*](../person-type/index.md) methods.

**Currency.** The `currency` field sets the currency in which the order is paid. The list of currencies is returned by the [crm.currency.list](../../crm/currency/crm-currency-list.md) method.

**Order Properties.** Properties are the data the buyer fills out when placing an order, such as "Metro Station" or "Date and Time of Delivery." Properties are created by the [sale.property.*](../property/index.md) methods, and their set depends on the payer type. Property values of a specific order are set by the [sale.propertyvalue.*](../property-value/index.md) methods, and the [sale.order.get](./sale-order-get.md) response returns them in `propertyValues`.

**Status.** The `statusId` field shows the order fulfillment stage. Statuses are created and modified by the [sale.status.*](../status/index.md) methods.

**Cart, Payments, and Shipments.** How these objects are linked to each other is shown in the "How an Order Is Structured" section of the Online Store [data types reference](../data-types.md).

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
|| [sale.order.delete](./sale-order-delete.md) | Deletes an order along with its basket items, payments, and shipments ||
|| [sale.order.getFields](./sale-order-get-fields.md) | Returns the description of order fields ||
|#