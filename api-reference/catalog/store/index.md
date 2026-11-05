# Warehouses in the Trade Catalog: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Warehouses help manage inventory, movements, and receipts of goods. You will always know where the necessary items are located and can timely distribute them among warehouses.

You can set access permissions for each warehouse. This way, employees will only see their own inventory documents and will not accidentally select another warehouse when creating new documents.

> Quick navigation: [all methods](#all-methods)
>
> User documentation:
> - [How to get started with inventory management](https://helpdesk.bitrix24.com/open/14836088/)
> - [Access permission for viewing and selecting a warehouse](https://helpdesk.bitrix24.com/open/16634838/)

## How to Start

1. Check available warehouse fields using [catalog.store.getFields](./catalog-store-get-fields.md)
2. Create a warehouse using [catalog.store.add](./catalog-store-add.md)
3. Retain the warehouse ID from the response — the `result.store.id` field. The IDs of existing warehouses are returned by [catalog.store.list](./catalog-store-list.md)
4. Use the warehouse identifier in inventory management documents or when reading inventory balances

You cannot delete a warehouse that is specified in inventory document items — delete those items or documents first. You cannot set a warehouse image via REST.

## Relationship with Other Objects

**Inventory Levels.** Find out how many items are left in the warehouse using the methods [catalog.storeproduct.*](../store-product/index.md).

**Inventory Management.** Add and modify inventory documents using the methods [catalog.document.*](../document/index.md). When adding products to a document with [catalog.document.element.add](../document/document-element/catalog-document-element-add.md), specify the IDs of the sender and recipient warehouses in the `storeFrom` and `storeTo` fields. You can obtain the warehouse IDs using the method [catalog.store.list](./catalog-store-list.md).

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the methods:
> - a user with the "View product catalog" or "View and select warehouses" access permission — for reading
> - a user with the "Edit warehouses" access permission — for creating, updating, and deleting

#|
|| **Method** | **Description** ||
|| [catalog.store.add](./catalog-store-add.md) | Adds a warehouse ||
|| [catalog.store.update](./catalog-store-update.md) | Modifies a warehouse ||
|| [catalog.store.get](./catalog-store-get.md) | Returns the field values of a warehouse by its ID ||
|| [catalog.store.list](./catalog-store-list.md) | Returns a list of warehouses based on a filter ||
|| [catalog.store.delete](./catalog-store-delete.md) | Deletes a warehouse ||
|| [catalog.store.getFields](./catalog-store-get-fields.md) | Returns the available fields of a warehouse ||
|#
