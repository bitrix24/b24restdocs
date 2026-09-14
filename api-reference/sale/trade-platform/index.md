# Order Sources in the Online Store: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Orders can be created manually using the [sale.order.add](../order/sale-order-add.md) method or obtained from internal sources:
- invoice,
- sales document,
- deal,
- activity,
- landing page.

To view all order sources in your Bitrix24, use the [sale.tradePlatform.list](./sale-trade-platform-list.md) method.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Online Store in Bitrix24: Getting Started](https://helpdesk.bitrix24.com/open/25809723/)

## Linking Order Sources to Other Objects

**Binding order sources to orders.** To view orders from a specific source, use the [sale.tradeBinding.list](../trade-binding/sale-trade-binding-list.md) method.

**Order.** Retrieve all information about an order using the [sale.order.get](../order/sale-order-get.md) method.

## How to Get Started

1. Retrieve the list of order sources using [sale.tradePlatform.list](./sale-trade-platform-list.md).
2. Retrieve the description of source fields using [sale.tradePlatform.getFields](./sale-trade-platform-get-fields.md).
3. Use the source ID in the filter of [sale.tradeBinding.list](../trade-binding/sale-trade-binding-list.md) to find linked orders.

## Overview of Methods {#all-methods}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: any user with the "View product catalog" access permission

#|
|| **Method** | **Description** ||
|| [sale.tradePlatform.list](./sale-trade-platform-list.md) | Returns a list of order sources ||
|| [sale.tradePlatform.getFields](./sale-trade-platform-get-fields.md) | Returns available fields for order sources ||
|#