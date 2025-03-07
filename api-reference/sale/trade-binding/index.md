# Linking Order Sources to Orders in the Online Store: Overview of Methods

Orders can be created manually using the [sale.order.add](../order/sale-order-add.md) method or obtained from internal sources:
- invoice,
- sales document,
- deal,
- activity,
- landing page.

To view orders from a specific source, use the [sale.tradeBinding.list](./sale-trade-binding-list.md) method.

## Connection of Order Source Bindings with Other Objects

**Order Sources.** Get information about all order sources in your Bitrix24 using the [sale.tradePlatform.list](../trade-platform/sale-trade-platform-list.md) method.

**Order.** View all information about an order using the [sale.order.get](../order/sale-order-get.md) method.

## Overview of Methods {#all-methods}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: any user with the "View product catalog" access permission

#|
|| **Method** | **Description** ||
|| [sale.tradeBinding.list](./sale-trade-binding-list.md) | Returns a list of orders from sources ||
|| [sale.tradeBinding.getFields](./sale-trade-binding-get-fields.md) | Returns available fields for orders from sources ||
|#