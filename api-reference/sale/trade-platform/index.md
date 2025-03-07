# Order Sources in the Online Store: Overview of Methods

Orders can be created manually using the [sale.order.add](../order/sale-order-add.md) method or obtained from internal sources:
- invoice,
- sales document,
- deal,
- activity,
- landing page.

To view all order sources in your Bitrix24, use the [sale.tradePlatform.list](./sale-trade-platform-list.md) method.

> Quick navigation: [all methods](#all-methods)

## Linking Order Sources to Other Objects

**Binding order sources to orders.** To view orders from a specific source, use the [sale.tradeBinding.list](../trade-binding/sale-trade-binding-list.md) method.

**Order.** Retrieve all information about an order using the [sale.order.get](../order/sale-order-get.md) method.

## Overview of Methods {#all-methods}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: any user with the "View product catalog" access permission

#|
|| **Method** | **Description** ||
|| [sale.tradePlatform.list](./sale-trade-platform-list.md) | Returns a list of order sources ||
|| [sale.tradePlatform.getFields](./sale-trade-platform-get-fields.md) | Returns available fields for order sources ||
|#