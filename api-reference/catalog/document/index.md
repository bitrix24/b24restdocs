# Inventory Accounting in the Trade Catalog: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Inventory accounting is a tool that allows you to track the available and reserved quantity of goods in warehouses.

To add, move, or remove goods from the warehouse, use inventory accounting documents.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Bitrix24 Inventory Management](https://helpdesk.bitrix24.com/open/14821994/)

## Inventory Accounting Documents

The following types of documents are available in inventory accounting:

- `A` — goods arrival at the warehouse
- `S` — goods receipt
- `M` — goods transfer between warehouses
- `R` — goods return
- `D` — goods write-off

Set access permissions for each type of document. If access permissions are not configured, employees will see an error when attempting to open inventory accounting documents and will be unable to work with them.

{% note tip "User Documentation" %}

- [Access permissions for Inventory management documents](https://helpdesk.bitrix24.com/open/25829011/)
- [Create a stock adjustment](https://helpdesk.bitrix24.com/open/22541392/)
- [Create a stock receipt](https://helpdesk.bitrix24.com/open/25801187/)
- [Work with sales orders](https://helpdesk.bitrix24.com/open/18570124/)
- [Write-offs](https://helpdesk.bitrix24.com/open/18044940/)
- [Transfers](https://helpdesk.bitrix24.com/open/23188106/)

{% endnote %}

## Current API Version

For new development, use the `catalog.document.*` and `catalog.document.element.*` methods. Deprecated methods [catalog.document.confirm](./outdated/catalog-document-confirm.md), [catalog.document.unconfirm](./outdated/catalog-document-unconfirm.md), [catalog.document.fields](./outdated/catalog-document-fields.md), and [catalog.document.element.fields](./outdated/catalog-document-element-fields.md) are kept only to support legacy integrations.

Current replacements:

#|
|| **If a Legacy Integration Uses** | **Use Now** ||
|| `catalog.document.confirm` | [catalog.document.conduct](./catalog-document-conduct.md) ||
|| `catalog.document.unconfirm` | [catalog.document.cancel](./catalog-document-cancel.md) ||
|| `catalog.document.fields` | [catalog.document.getFields](./catalog-document-get-fields.md) ||
|| `catalog.document.element.fields` | [catalog.document.element.getFields](./document-element/catalog-document-element-get-fields.md) ||
|#

## Inventory Accounting in REST 3.0

In [REST 3.0](../../rest-v3.md), inventory accounting documents have methods for adding, updating, retrieving, deleting, conducting, and canceling the conduct, inventory accounting mode methods, the [catalog.document.type.list](./catalog-document-type-list.md) directory of document types, and the `*.field.list` and `*.field.get` methods for retrieving the field schema. Methods available in both API versions are marked `v 3.0` in the tables.

Specifics of the new version methods:

- the [catalog.document.conduct](./catalog-document-conduct-rest-v3.md) and [catalog.document.cancel](./catalog-document-cancel-rest-v3.md) methods accept the `id` of a single document or a `documentIds` array. The batch is not rolled back: documents without errors are conducted, and for the rest the method returns an error
- the `catalog.document.confirm` and `catalog.document.unconfirm` methods in the new version repeat `conduct` and `cancel` for a single document
- document permissions are checked by document type: "View", "Create and edit", "Process inventory objects", "Cancel processing", "Delete inventory object". The [catalog.document.list](./catalog-document-list-rest-v3.md) method returns only documents of the types the user can view
- the adding, updating, and deleting methods are unavailable if inventory accounting is not included in the plan
- in the response, the `status` field of a conducted document is `"1"`, and of an unconducted document — an empty string
- the method names `deletelist`, `conductlist`, `cancellist`, and `mode.statusdetails` are written only in lowercase

## Linking Inventory Accounting Documents with Other Objects

**Goods in the Inventory Accounting Document.** Specify the goods for the inventory accounting document using the methods [catalog.document.element.*](./document-element/index.md).

**Warehouses.** Specify the warehouse for which you are creating the inventory accounting document. Use the methods [catalog.store.*](../store/index.md).

**Custom Fields for Inventory Accounting Documents.** You can create additional fields for inventory accounting documents using the method [userfieldconfig.add](../../crm/universal/userfieldconfig/userfieldconfig-add.md). Pass `moduleId = catalog`, and specify the document type in `entityId` using the `CAT_STORE_DOCUMENT_<DOC_TYPE>` format, for example `CAT_STORE_DOCUMENT_A`. You can view additional fields and change their values using the methods [catalog.userfield.document.*](../userfield-document/index.md).

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute methods:
> - administrator — for methods of the previous API version
> - a user with permissions for documents of the required type — for REST 3.0 methods. The permissions are listed on the page of each method

### Basic

#| 
|| **Method** | **Description** ||
|| [catalog.document.add](./catalog-document-add.md) | Adds an inventory accounting document ||
|| [catalog.document.add](./catalog-document-add-rest-v3.md) | Adds an inventory accounting document v 3.0 ||
|| [catalog.document.update](./catalog-document-update.md) | Updates an inventory accounting document ||
|| [catalog.document.update](./catalog-document-update-rest-v3.md) | Updates an inventory accounting document v 3.0 ||
|| [catalog.document.list](./catalog-document-list.md) | Returns a list of inventory accounting documents ||
|| [catalog.document.list](./catalog-document-list-rest-v3.md) | Returns a list of inventory accounting documents v 3.0 ||
|| [catalog.document.get](./catalog-document-get.md) | Returns an inventory accounting document by identifier ||
|| [catalog.document.delete](./catalog-document-delete.md) | Deletes an inventory accounting document ||
|| [catalog.document.delete](./catalog-document-delete-rest-v3.md) | Deletes an inventory accounting document v 3.0 ||
|| [catalog.document.deleteList](./catalog-document-delete-list.md) | Deletes a group of inventory accounting documents ||
|| [catalog.document.deletelist](./catalog-document-delete-list-rest-v3.md) | Deletes a group of inventory accounting documents v 3.0 ||
|| [catalog.document.conduct](./catalog-document-conduct.md) | Conducts an inventory accounting document ||
|| [catalog.document.conduct](./catalog-document-conduct-rest-v3.md) | Conducts an inventory accounting document v 3.0 ||
|| [catalog.document.conductList](./catalog-document-conduct-list.md) | Conducts a group of inventory accounting documents ||
|| [catalog.document.conductlist](./catalog-document-conduct-list-rest-v3.md) | Conducts a group of inventory accounting documents v 3.0 ||
|| [catalog.document.cancel](./catalog-document-cancel.md) | Cancels the conduct of an inventory accounting document ||
|| [catalog.document.cancel](./catalog-document-cancel-rest-v3.md) | Cancels the conduct of an inventory accounting document v 3.0 ||
|| [catalog.document.cancelList](./catalog-document-cancel-list.md) | Cancels the conduct of a group of inventory accounting documents ||
|| [catalog.document.cancellist](./catalog-document-cancel-list-rest-v3.md) | Cancels the conduct of a group of inventory accounting documents v 3.0 ||
|| [catalog.document.confirm](./catalog-document-confirm-rest-v3.md) | Conducts a single inventory accounting document v 3.0 ||
|| [catalog.document.unconfirm](./catalog-document-unconfirm-rest-v3.md) | Cancels the conduct of a single document v 3.0 ||
|| [catalog.document.mode.status](./catalog-document-mode-status.md) | Checks if inventory accounting is enabled ||
|| [catalog.document.mode.status](./catalog-document-mode-status-rest-v3.md) | Checks if inventory accounting is enabled v 3.0 ||
|| [catalog.document.mode.statusdetails](./catalog-document-mode-status-details.md) | Returns the inventory accounting status and mode ||
|| [catalog.document.mode.field.list](./catalog-document-mode-field-list.md) | Returns a list of inventory accounting status fields v 3.0 ||
|| [catalog.document.mode.field.get](./catalog-document-mode-field-get.md) | Returns the description of an inventory accounting status field ||
|| [catalog.document.getFields](./catalog-document-get-fields.md) | Returns available fields of the inventory accounting document ||
|| [catalog.document.field.list](./catalog-document-field-list.md) | Returns a list of inventory accounting document fields v 3.0 ||
|| [catalog.document.field.get](./catalog-document-field-get.md) | Returns the description of an inventory accounting document field ||
|| [catalog.document.type.list](./catalog-document-type-list.md) | Returns inventory accounting document types ||
|| [catalog.document.type.field.list](./catalog-document-type-field-list.md) | Returns a list of document type fields v 3.0 ||
|| [catalog.document.type.field.get](./catalog-document-type-field-get.md) | Returns the description of a document type field ||
|#

### Goods in the Inventory Accounting Document

#| 
|| **Method** | **Description** ||
|| [catalog.document.element.add](./document-element/catalog-document-element-add.md) | Adds a good to the inventory accounting document ||
|| [catalog.document.element.add](./document-element/catalog-document-element-add-rest-v3.md) | Adds a good to the inventory accounting document v 3.0 ||
|| [catalog.document.element.update](./document-element/catalog-document-element-update.md) | Updates a good in the inventory accounting document ||
|| [catalog.document.element.update](./document-element/catalog-document-element-update-rest-v3.md) | Updates a good in the inventory accounting document v 3.0 ||
|| [catalog.document.element.list](./document-element/catalog-document-element-list.md) | Returns a list of goods in the inventory accounting document ||
|| [catalog.document.element.list](./document-element/catalog-document-element-list-rest-v3.md) | Returns a list of goods in the inventory accounting document v 3.0 ||
|| [catalog.document.element.delete](./document-element/catalog-document-element-delete.md) | Deletes a good from the inventory accounting document ||
|| [catalog.document.element.delete](./document-element/catalog-document-element-delete-rest-v3.md) | Deletes a good from the inventory accounting document v 3.0 ||
|| [catalog.document.element.getFields](./document-element/catalog-document-element-get-fields.md) | Returns a list of available fields for goods in the inventory accounting document ||
|| [catalog.document.element.field.list](./document-element/catalog-document-element-field-list.md) | Returns a list of product fields for the inventory accounting document v 3.0 ||
|| [catalog.document.element.field.get](./document-element/catalog-document-element-field-get.md) | Returns the description of a product field in the inventory accounting document ||
|#
