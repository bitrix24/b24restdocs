# Products in Inventory Management Document: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The methods `catalog.document.element.*` add, modify, or remove product items in inventory management documents. You can specify the quantity and purchase price of the product, as well as the warehouse for receipt or write-off.

> Quick navigation: [all methods](#all-methods)
>
> User documentation:
> - [Create a Stock Receipt](https://helpdesk.bitrix24.com/open/25801187/)
> - [Transfers](https://helpdesk.bitrix24.com/open/23188106/)

## How to Start

1. Create an unprocessed inventory management document using the [catalog.document.add](../catalog-document-add.md) method or find one using the [catalog.document.list](../catalog-document-list.md) method
2. Get the `id` of a product or variation
3. Get the `id` of the receipt or write-off warehouse — the field depends on the document type, see the list below
4. Add a product item using [catalog.document.element.add](./catalog-document-element-add.md): pass `docId`, `elementId`, the warehouse, the quantity `amount`, and the purchase price `purchasingPrice`
5. Check document items using [catalog.document.element.list](./catalog-document-element-list.md) with a filter by `docId`

The methods do not cross-check the product, the item document, and the warehouses: they do not verify that the product and warehouses exist or that the warehouse matches the document type. Check the identifiers before the call.

## Relationship with Other Objects

**Inventory Management Documents.** Specify the document ID in the `docId` field to process:
- product receipt to the warehouse
- stock adjustment of the product
- transfer of the product between warehouses
- product return
- write-off of the product from the warehouse

To obtain the document ID, use the [catalog.document.list](../catalog-document-list.md) method. You can add, update, and delete items only in a document that has not been processed yet. The product and the document of an item are set only when the item is added: to replace the product, delete the item and add a new one.

**Catalog Products.** Specify the ID of the simple product or variation in the `elementId` field.
Services are not supported in inventory management.

To obtain product IDs, use the methods:
- [catalog.product.list](../../product/catalog-product-list.md) for simple products
- [catalog.product.offer.list](../../product/offer/catalog-product-offer-list.md) for variations

**Warehouses.** Specify the ID of the receipt warehouse in the `storeTo` field or the write-off warehouse in the `storeFrom` field. Choose the field depending on the type of inventory management document:

- receipt — `storeTo`
- stock adjustment — `storeTo`
- transfer — `storeFrom` and `storeTo`
- return — `storeTo`
- write-off — `storeFrom`

To obtain warehouse IDs, use the [catalog.store.list](../../store/catalog-store-list.md) method.

## Document Products in REST 3.0

In [REST 3.0](../../../rest-v3.md), document products have methods for adding, updating, retrieving a list, and deleting, as well as the `catalog.document.element.field.list` and `catalog.document.element.field.get` methods for retrieving the field schema. Methods available in both API versions are marked `v 3.0` in the table.

The adding, updating, and deleting methods work only with an unprocessed document. To change the products of a processed document, first cancel its processing using the [catalog.document.cancel](../catalog-document-cancel-rest-v3.md) method.

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can perform the methods:
> - `add`, `update`, `delete` — a user with the "Create and edit" permission for documents of this type and the "View and select warehouses" permission for the item warehouses
> - `list`, `getFields` — a user with the "View product catalog" or "View and select warehouses" permission. The list includes items of documents whose type the user has the "View" permission for, and of available warehouses
> - `field.list`, `field.get` — any user

#|
|| **Method** | **Description** ||
|| [catalog.document.element.add](./catalog-document-element-add.md) | Adds a product item to the inventory management document ||
|| [catalog.document.element.add](./catalog-document-element-add-rest-v3.md) | Adds a product item to the inventory management document v 3.0 ||
|| [catalog.document.element.update](./catalog-document-element-update.md) | Modifies an existing product item in the inventory management document ||
|| [catalog.document.element.update](./catalog-document-element-update-rest-v3.md) | Modifies an existing product item in the inventory management document v 3.0 ||
|| [catalog.document.element.list](./catalog-document-element-list.md) | Returns product items of documents by filter ||
|| [catalog.document.element.list](./catalog-document-element-list-rest-v3.md) | Returns product items of documents by filter v 3.0 ||
|| [catalog.document.element.delete](./catalog-document-element-delete.md) | Removes a product item from the document before processing ||
|| [catalog.document.element.delete](./catalog-document-element-delete-rest-v3.md) | Removes a product item from the document before processing v 3.0 ||
|| [catalog.document.element.getFields](./catalog-document-element-get-fields.md) | Returns the description of product fields from the inventory management document ||
|| [catalog.document.element.field.list](./catalog-document-element-field-list.md) | Returns a list of product fields for the inventory management document v 3.0 ||
|| [catalog.document.element.field.get](./catalog-document-element-field-get.md) | Returns the description of a product field in the inventory management document ||
|#
