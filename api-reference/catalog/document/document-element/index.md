# Products in Inventory Management Document: Overview of Methods

The methods `catalog.document.element.*` add, modify, or remove product items in inventory management documents. You can specify the quantity and purchase price of the product, as well as the warehouse for receipt or write-off.

> Quick navigation: [all methods](#all-methods)

## Connection with Other Objects

**Inventory Management Documents.** Specify the document ID in the `docId` field to process:
- product receipt to the warehouse,
- stock adjustment of the product,
- transfer of the product between warehouses,
- product return,
- write-off of the product from the warehouse.

To obtain the document ID, use the [catalog.document.list](../catalog-document-list.md) method. Changes to the list of product items can only be made in a document that has not yet been processed.

**Catalog Products.** Specify the ID of the simple product or variation in the `elementId` field. 
Services are not supported in inventory management.

To obtain product IDs, use the methods:
- [catalog.product.list](../../product/catalog-product-list.md) for simple products,
- [catalog.product.offer.list](../../product/offer/catalog-product-offer-list.md) for variations.

**Warehouses.** Specify the ID of the receipt warehouse in the `storeTo` field or the write-off warehouse in the `storeFrom` field. Choose the field depending on the type of inventory management document:

- receipt — `storeTo`,
- stock adjustment — `storeTo`,
- transfer — `storeTo` and `storeFrom`,
- return — `storeFrom`,
- write-off — `storeFrom`.

To obtain warehouse IDs, use the [catalog.store.list](../../store/catalog-store-list.md) method.

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can perform the methods: depending on the method

#|
|| **Method** | **Description** ||
|| [catalog.document.element.add](./catalog-document-element-add.md) | Adds a product item to the inventory management document ||
|| [catalog.document.element.update](./catalog-document-element-update.md) | Modifies an existing product item in the inventory management document ||
|| [catalog.document.element.list](./catalog-document-element-list.md) | Returns product items of documents by filter ||
|| [catalog.document.element.delete](./catalog-document-element-delete.md) | Removes a product item from the document before processing ||
|| [catalog.document.element.getFields](./catalog-document-element-get-fields.md) | Returns the description of product fields from the inventory management document ||
|#