# Order Sources in the Online Store: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

An order source shows where an order came from: a deal, an invoice, a landing page, a CRM form, or another Bitrix24 tool. Bitrix24 creates order sources, and the methods in this section only read them. Source codes are described on the [sale.tradePlatform.list](./sale-trade-platform-list.md#codes) method page.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Online Store in Bitrix24: Getting Started](https://helpdesk.bitrix24.com/open/25809723/)

## How to Get Started

1. Retrieve the list of order sources using [sale.tradePlatform.list](./sale-trade-platform-list.md)
2. Retrieve the description of source fields using [sale.tradePlatform.getFields](./sale-trade-platform-get-fields.md)
3. Use the source ID in the filter of [sale.tradeBinding.list](../trade-binding/sale-trade-binding-list.md) to retrieve the order bindings of that source

## Linking Order Sources to Other Objects

**Binding orders to sources.** A binding references a source through the `tradingPlatformId` field, which holds the source `id`. The [sale.tradeBinding.list](../trade-binding/sale-trade-binding-list.md) method retrieves the order bindings of a source.

**Order.** Retrieve all information about an order using the [sale.order.get](../order/sale-order-get.md) method.

## Limitations and Specifics

- The methods in this section are read-only: you cannot create, update, or delete an order source via REST
- The list also includes inactive sources. The `active` field shows whether a source is active; filter by it: `{"active": "Y"}`

## Overview of Methods {#all-methods}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the methods: `list` — user with access to the Online Store, `getFields` — any user

#|
|| **Method** | **Description** ||
|| [sale.tradePlatform.list](./sale-trade-platform-list.md) | Returns a list of order sources ||
|| [sale.tradePlatform.getFields](./sale-trade-platform-get-fields.md) | Returns available fields for order sources ||
|#