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
2. Get the product or variation identifier using [catalog.product.list](../product/catalog-product-list.md) or [catalog.product.offer.list](../product/offer/catalog-product-offer-list.md)
3. Get inventory balances using [catalog.storeproduct.list](./catalog-store-product-list.md) with a filter by `productId` and `storeId`. The [catalog.storeproduct.get](./catalog-store-product-get.md) method takes the inventory balance record identifier, not the product identifier
4. To change inventory balances, use inventory management documents through [catalog.document.*](../document/index.md)

## Inventory Balances in REST 3.0

In [REST 3.0](../../rest-v3.md), method names have a dot between `store` and `product`: `catalog.store.product.*` instead of the previous `catalog.storeproduct.*`. Inventory balances can be retrieved as a list or by the record identifier. The `catalog.store.product.field.list` and `catalog.store.product.field.get` methods return the description of inventory balance fields. Methods available in both API versions are marked `v 3.0` in the table.

Specifics of the new version methods:

- the filter is passed in the REST 3.0 format: an array of conditions of the form `["field", "operator", "value"]`, for example `[["productId", "=", 6595]]`
- the total number of records is not returned. If the response contains fewer records than `limit`, the page is the last one
- as in the previous version, inventory balances are read-only. They can be changed with inventory management documents

## Relationship with Other Objects

**Inventories.** To select inventory balances at a specific warehouse, specify the warehouse identifier in the `storeId` field of the [catalog.storeproduct.list](./catalog-store-product-list.md) method filter. You can retrieve the warehouse identifier using the [catalog.store.list](../store/catalog-store-list.md) method.

**Catalog products.** To select inventory balances for a specific product, specify the product identifier in the `productId` field of the [catalog.storeproduct.list](./catalog-store-product-list.md) method filter. You can retrieve the product identifier using the following methods:

- [catalog.product.list](../product/catalog-product-list.md) — for simple products
- [catalog.product.offer.list](../product/offer/catalog-product-offer-list.md) — for variations

**Inventory management documents.** The `catalog.storeproduct.*` methods only read inventory balances. To change inventory balances, use inventory management documents in the [catalog.document.*](../document/index.md) methods.

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the methods:
> - reading inventory balances — a user with the "View product catalog" or "View and select warehouses" access permission. The methods return balances only for the warehouses available to the user under the "View and select warehouses" permission
> - `catalog.store.product.field.list` and `catalog.store.product.field.get` — any user

#|
|| **Method** | **Description** ||
|| [catalog.storeproduct.get](./catalog-store-product-get.md) | Returns a product inventory balance record in a warehouse by the record identifier ||
|| [catalog.store.product.get](./catalog-store-product-get-rest-v3.md) | Returns a product inventory balance record in a warehouse by the record identifier v 3.0 ||
|| [catalog.storeproduct.list](./catalog-store-product-list.md) | Returns a list of inventory balances based on the filter ||
|| [catalog.store.product.list](./catalog-store-product-list-rest-v3.md) | Returns a list of inventory balances based on the filter v 3.0 ||
|| [catalog.storeproduct.getFields](./catalog-store-product-get-fields.md) | Returns the fields of inventory balances ||
|| [catalog.store.product.field.list](./catalog-store-product-field-list.md) | Returns a list of inventory balance fields v 3.0 ||
|| [catalog.store.product.field.get](./catalog-store-product-field-get.md) | Returns the description of an inventory balance field ||
|#
