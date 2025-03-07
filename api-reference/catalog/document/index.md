# Inventory management in the Trade Catalog: Overview of Methods

Inventory management is a tool that allows you to track the available and reserved quantity of products in warehouses.

To add, move, or remove products from the inventory, use inventory management documents.

> Quick navigation: [all methods](#all-methods)
> 
> User documentation: 
> - [Inventory management in Bitrix24](https://helpdesk.bitrix24.com/open/14821994/)
> - [How to get started with inventory management](https://helpdesk.bitrix24.com/open/14836088/)

## Inventory Management Documents

The following types of documents are available in inventory management:
- `A` – stock receipt of products,
- `S` – stock adjustment of products,
- `M` – transfer of products between warehouses,
- `R` – return of products,
- `D` – write-off of products.

Set access permissions for each type of document. If access permissions are not configured, an employee will see an error when trying to open inventory management documents and will not be able to work with them.

{% note tip "User documentation" %}

- [Access permissions Inventory management documents](https://helpdesk.bitrix24.com/open/16103170/)
- [Work with sales orders](https://helpdesk.bitrix24.com/open/18570124/)
- [Create a stock receipt](https://helpdesk.bitrix24.com/open/22619814/)
- [Create a stock adjustment](https://helpdesk.bitrix24.com/open/22541392/)
- [Reason for write-off](https://helpdesk.bitrix24.com/open/18043598/)
- [Print Inventory Management documents](https://helpdesk.bitrix24.com/open/15832418/)
- [Write-offs](https://helpdesk.bitrix24.com/open/18044940/)
- [Transfers](https://helpdesk.bitrix24.com/open/23188106/)

{% endnote %}

## Connection of Inventory Management Documents with Other Objects

**Products of the inventory management document.** Specify products for the inventory management document using the methods [catalog.document.element.*](./document-element/index.md).

**Warehouses.** Specify the warehouse for which you are creating the inventory management document. Use the methods [catalog.store.*](../store/index.md).

**Custom fields of inventory management documents.** You can create additional fields for inventory management documents using the method [userfieldconfig.add](../../crm/universal/userfieldconfig/userfieldconfig/userfieldconfig-add.md), where `moduleId` — catalog, and `entityId` — CAT_STORE_DOCUMENT_DocumentTypeIdentifier. To view additional fields or change their values, use the methods [catalog.userfield.document.*](../userfield-document/index.md).

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute methods: administrator

### Main

#|
|| **Method** | **Description** ||
|| [catalog.document.mode.status](./catalog-document-mode-status.md) | Checks if inventory management is enabled ||
|| [catalog.document.add](./catalog-document-add.md) | Adds an inventory management document ||
|| [catalog.document.conduct](./catalog-document-conduct.md) | Conducts an inventory management document ||
|| [catalog.document.conductList](./catalog-document-conduct-list.md) | Conducts a group of inventory management documents ||
|| [catalog.document.cancel](./catalog-document-cancel.md) | Cancels the conduct of an inventory management document by its identifier ||
|| [catalog.document.cancelList](./catalog-document-cancel-list.md) | Cancels the conduct of a group of inventory management documents ||
|| [catalog.document.update](./catalog-document-update.md) | Modifies an inventory management document ||
|| [catalog.document.list](./catalog-document-list.md) | Returns a list of inventory management documents ||
|| [catalog.document.delete](./catalog-document-delete.md) | Deletes an inventory management document ||
|| [catalog.document.deleteList](./catalog-document-delete-list.md) | Deletes a group of inventory management documents ||
|| [catalog.document.getFields](./catalog-document-get-fields.md) | Returns available fields of the inventory management document ||
|#

### Products of the Inventory Management Document

#|
|| **Method** | **Description** ||
|| [catalog.document.element.add](./document-element/catalog-document-element-add.md) | Adds a product to the inventory management document ||
|| [catalog.document.element.update](./document-element/catalog-document-element-update.md) | Modifies a product in the inventory management document ||
|| [catalog.document.element.list](./document-element/catalog-document-element-list.md) | Returns a list of products in the inventory management document ||
|| [catalog.document.element.delete](./document-element/catalog-document-element-delete.md) | Deletes a product from the inventory management document ||
|| [catalog.document.element.getFields](./document-element/catalog-document-element-get-fields.md) | Returns a list of available fields for products in the inventory management document ||
|#