# Inventory balances: overview of methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Inventory balances indicate how much product is available at each inventory location. The methods `catalog.storeproduct.*` assist in:

- synchronizing inventory balances between Bitrix24 and external systems,
- tracking product movement
- preventing sales when a product is out of stock

> Quick navigation: [all methods](#all-methods)

## How to Start

1. Get the warehouse identifier using [catalog.store.list](../store/catalog-store-list.md)
2. Get the product identifier using the required product method group
3. Get inventory balances using [catalog.storeproduct.get](./catalog-store-product-get.md) or [catalog.storeproduct.list](./catalog-store-product-list.md)
4. To change inventory balances, use inventory management documents through [catalog.document.*](../document/index.md)

## Relationship with Other Objects

**Inventories.** To select inventory balances at a specific location, specify the inventory location's `ID` in the filter of the method [catalog.storeproduct.list](./catalog-store-product-list.md). The inventory location's `ID` can be obtained using the method [catalog.store.list](../store/catalog-store-list.md).

**Catalog products.** To select inventory balances for a specific product, specify the product's `ID` in the filter of the method [catalog.storeproduct.list](./catalog-store-product-list.md). The product's `ID` can be obtained using the methods:

- [catalog.product.list](../product/catalog-product-list.md) — for simple products
- [catalog.product.offer.list](../product/offer/catalog-product-offer-list.md) — for variations

**Inventory management documents.** To modify inventory balances, use inventory management documents in the methods [catalog.document.*](../document/index.md).

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the methods: a user with the "View product catalog" access permission

#|
|| **Method** | **Description** ||
|| [catalog.storeproduct.get](./catalog-store-product-get.md) | Returns the inventory balance record for a specific identifier ||
|| [catalog.storeproduct.list](./catalog-store-product-list.md) | Returns a list of inventory balances based on the filter ||
|| [catalog.storeproduct.getFields](./catalog-store-product-get-fields.md) | Returns the fields of inventory balances ||
|#
