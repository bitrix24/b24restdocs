# Inventory Accounting in the Trade Catalog: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Inventory accounting is a tool that allows you to track the available and reserved quantity of goods in warehouses.

To add, move, or remove goods from the warehouse, use inventory accounting documents.

> Quick navigation: [all methods](#all-methods)
> 
> User documentation: 
> - [Bitrix24 Inventory Management](https://helpdesk.bitrix24.com/open/14821994/)

## Inventory Accounting Documents

The following types of documents are available in inventory accounting:
- `A` – goods arrival at the warehouse,
- `S` – goods receipt,
- `M` – goods transfer between warehouses,
- `R` – goods return,
- `D` – goods write-off.

Set access permissions for each type of document. If access permissions are not configured, employees will see an error when attempting to open inventory accounting documents and will be unable to work with them.

{% note tip "User Documentation" %}

- [Access permissions for Inventory management documents](https://helpdesk.bitrix24.com/open/25829011/)
- [Create a stock adjustment](https://helpdesk.bitrix24.com/open/22541392/)
- [Create a stock receipt](https://helpdesk.bitrix24.com/open/25801187/)
- [Work with sales orders](https://helpdesk.bitrix24.com/open/18570124/)
- [Write-offs](https://helpdesk.bitrix24.com/open/18044940/)
- [Transfers](https://helpdesk.bitrix24.com/open/23188106/)

{% endnote %}

## Linking Inventory Accounting Documents with Other Objects

**Goods in the Inventory Accounting Document.** Specify the goods for the inventory accounting document using the methods [catalog.document.element.*](./document-element/index.md).

**Warehouses.** Specify the warehouse for which you are creating the inventory accounting document. Use the methods [catalog.store.*](../store/index.md).

**Custom Fields for Inventory Accounting Documents.** You can create additional fields for inventory accounting documents using the method [userfieldconfig.add](../../crm/universal/userfieldconfig/userfieldconfig-add.md), where `moduleId` is catalog, and `entityId` is CAT_STORE_DOCUMENT_DocumentTypeIdentifier. To view additional fields or change their values, use the methods [catalog.userfield.document.*](../userfield-document/index.md).

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute methods: administrator

### Basic

#| 
|| **Method** | **Description** ||
|| [catalog.document.mode.status](./catalog-document-mode-status.md) | Checks if inventory accounting is enabled ||
|| [catalog.document.add](./catalog-document-add.md) | Adds an inventory accounting document ||
|| [catalog.document.conduct](./catalog-document-conduct.md) | Conducts an inventory accounting document ||
|| [catalog.document.conductList](./catalog-document-conduct-list.md) | Conducts a group of inventory accounting documents ||
|| [catalog.document.cancel](./catalog-document-cancel.md) | Cancels the conduct of an inventory accounting document by its identifier ||
|| [catalog.document.cancelList](./catalog-document-cancel-list.md) | Cancels the conduct of a group of inventory accounting documents ||
|| [catalog.document.update](./catalog-document-update.md) | Updates an inventory accounting document ||
|| [catalog.document.list](./catalog-document-list.md) | Returns a list of inventory accounting documents ||
|| [catalog.document.delete](./catalog-document-delete.md) | Deletes an inventory accounting document ||
|| [catalog.document.deleteList](./catalog-document-delete-list.md) | Deletes a group of inventory accounting documents ||
|| [catalog.document.getFields](./catalog-document-get-fields.md) | Returns available fields of the inventory accounting document ||
|#

### Goods in the Inventory Accounting Document

#| 
|| **Method** | **Description** ||
|| [catalog.document.element.add](./document-element/catalog-document-element-add.md) | Adds a good to the inventory accounting document ||
|| [catalog.document.element.update](./document-element/catalog-document-element-update.md) | Updates a good in the inventory accounting document ||
|| [catalog.document.element.list](./document-element/catalog-document-element-list.md) | Returns a list of goods in the inventory accounting document ||
|| [catalog.document.element.delete](./document-element/catalog-document-element-delete.md) | Deletes a good from the inventory accounting document ||
|| [catalog.document.element.getFields](./document-element/catalog-document-element-get-fields.md) | Returns a list of available fields for goods in the inventory accounting document ||
|#