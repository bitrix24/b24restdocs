# Binding Orders to Sources in the Online Store: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A binding is a record that links an order to its source. The `sale.tradeBinding.*` methods return bindings and the description of their fields. You cannot create or modify a binding with these methods.

Order sources include invoices, sales orders, deals, activities, and landing pages. Their list is returned by the methods of the [Order Sources](../trade-platform/index.md) section.

> Quick navigation: [All Methods](#all-methods)

## Getting Started

1. Retrieve a list of order sources using the [sale.tradePlatform.list](../trade-platform/sale-trade-platform-list.md) method
2. Check available binding fields using the [sale.tradeBinding.getFields](./sale-trade-binding-get-fields.md) method
3. Retrieve the order bindings of the required source using the [sale.tradeBinding.list](./sale-trade-binding-list.md) method
4. Retrieve data for a specific order using the [sale.order.get](../order/sale-order-get.md) method

## Connection with Other Objects

Each binding is linked to two objects: an order source and an order.

**Order Source.** A binding references a source through the `tradingPlatformId` field, which is the `id` from the `sale.tradePlatform.list` response.

**Order.** A binding references an order through the `orderId` field. Pass this value to the `id` parameter of the `sale.order.get` method to retrieve the order contents and status.

The `sale.order.get` method itself returns the order bindings in the `tradeBindings` field, so you can find the source of a known order without calling `sale.tradeBinding.list`.

## Overview of Methods {#all-methods}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#|
|| **Method** | **Description** ||
|| [sale.tradeBinding.list](./sale-trade-binding-list.md) | Returns a list of order bindings to sources ||
|| [sale.tradeBinding.getFields](./sale-trade-binding-get-fields.md) | Returns the fields of an order binding to a source ||
|#
