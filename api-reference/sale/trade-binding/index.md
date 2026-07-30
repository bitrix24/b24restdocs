# Linking Order Sources to Orders in the Online Store: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Orders can be created manually using the [sale.order.add](../order/sale-order-add.md) method or obtained from internal sources:

- invoice
- sales order
- deal
- activity
- landing page

To view orders from a specific source, use the [sale.tradeBinding.list](./sale-trade-binding-list.md) method.

> Quick navigation: [All Methods](#all-methods)

## Getting Started

1. Retrieve a list of order sources using the [sale.tradePlatform.list](../trade-platform/sale-trade-platform-list.md) method
2. Check available binding fields using the [sale.tradeBinding.getFields](./sale-trade-binding-get-fields.md) method
3. Retrieve a list of orders by source using the [sale.tradeBinding.list](./sale-trade-binding-list.md) method
4. Retrieve data for a specific order using the [sale.order.get](../order/sale-order-get.md) method

## Order Source Identifiers

- `tradingPlatformId` — the order source identifier. You can retrieve a list of sources and their identifiers using the [sale.tradePlatform.list](../trade-platform/sale-trade-platform-list.md) method
- `orderId` — the identifier of the order linked to the source. Detailed order data can be retrieved using the [sale.order.get](../order/sale-order-get.md) method
- Available binding fields that can be used in `select`, `filter`, and `order` are returned by the [sale.tradeBinding.getFields](./sale-trade-binding-get-fields.md) method

## Connection with Other Objects

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
