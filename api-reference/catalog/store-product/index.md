# Inventory balances: overview of methods

Inventory balances indicate how much product is available at each inventory location. The methods `catalog.storeproduct.*` assist in:

- synchronizing inventory balances between Bitrix24 and external systems,

- tracking product movement,

- preventing sales when a product is out of stock.

> Quick navigation: [all methods](#all-methods)

## Connection with other objects

**Inventories.** To select inventory balances at a specific location, specify the inventory location's `ID` in the filter of the method [catalog.storeproduct.list](./catalog-store-product-list.md). The inventory location's `ID` can be obtained using the method [catalog.store.list](../store/catalog-store-list.md).

**Catalog products.** To select inventory balances for a specific product, specify the product's `ID` in the filter of the method [catalog.storeproduct.list](./catalog-store-product-list.md). The product's `ID` can be obtained using the methods:

- [catalog.product.list](../product/catalog-product-list.md) — for simple products,

- [catalog.product.offer.list](../product/offer/catalog-product-offer-list.md) — for variations.

**Inventory management documents.** To modify inventory balances, use inventory management documents in the methods [catalog.document.*](../document/index.md).

## Overview of methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" access permission

#|
|| **Method** | **Description** ||
|| [catalog.storeproduct.list](./catalog-store-product-list.md) | Returns a list of inventory balances based on the filter ||
|| [catalog.storeproduct.get](./catalog-store-product-get.md) | Returns the inventory balance record for a specific identifier ||
|| [catalog.storeproduct.getFields](./catalog-store-product-get-fields.md) | Returns the fields of inventory balances ||
|#