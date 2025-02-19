# Warehouses in the Trade Catalog: Overview of Methods

Warehouses help manage inventory, movements, and receipts of goods. You will always know where the necessary items are located and can timely distribute them among warehouses.

You can set access permissions for each warehouse. This way, employees will only see their own inventory documents and will not accidentally select another warehouse when creating new documents.

> Quick navigation: [all methods](#all-methods)
> 
> User documentation: 
> - [How to get started with inventory management](https://helpdesk.bitrix24.com/open/14836088/)
> - [Access permission for viewing and selecting a warehouse](https://helpdesk.bitrix24.com/open/16634838/)

## Connection of Warehouses with Other Objects

**Inventory Levels.** Find out how many items are left in the warehouse using the methods [catalog.storeproduct.*](../store-product/index.md).

**Inventory Management.** Add and modify inventory documents using the methods [catalog.document.*](../document/index.md). When creating documents, specify the `storeFrom` and `storeTo` parameters with the IDs of the sender and recipient warehouses. You can obtain the warehouse IDs using the method [catalog.store.list](./catalog-store-list.md).

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

#|
|| **Method** | **Description** ||
|| [catalog.store.add](./catalog-store-add.md) | Adds a warehouse ||
|| [catalog.store.update](./catalog-store-update.md) | Modifies a warehouse ||
|| [catalog.store.get](./catalog-store-get.md) | Returns the field values of a warehouse by its ID ||
|| [catalog.store.list](./catalog-store-list.md) | Returns a list of warehouses based on a filter ||
|| [catalog.store.delete](./catalog-store-delete.md) | Deletes a warehouse ||
|| [catalog.store.getFields](./catalog-store-get-fields.md) | Returns the available fields of a warehouse ||
|#