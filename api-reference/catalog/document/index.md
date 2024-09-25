# Methods for Managing Inventory

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can perform the method: any user

Methods for managing inventory:

#|
|| **Method** | **Description** | **Note** ||
|| [catalog.document.add](./catalog-document-add.md) | This method adds an inventory document. | ||
|| [catalog.document.cancel](./catalog-document-cancel.md) | This method cancels the processing of an inventory document by ID. | ||
|| [catalog.document.cancelList](./catalog-document-cancel-list.md) | This method cancels the processing of a group of inventory documents. | ||
|| [catalog.document.conduct](./catalog-document-conduct.md) | This method is used to process an inventory document. | ||
|| [catalog.document.conductList](./catalog-document-conduct-list.md) | This method is used for bulk processing of inventory documents. | ||
|| [catalog.document.confirm](./catalog-document-confirm.md) | This method is for processing a document. | This method is deprecated since version **22.400.0**. It is recommended to use the method [catalog.document.conduct](./catalog-document-conduct.md).||
|| [catalog.document.delete](./catalog-document-delete.md) | This method deletes an inventory document. | ||
|| [catalog.document.deleteList](./catalog-document-delete-list.md) | This method is used for bulk deletion of inventory documents. | ||
|| [catalog.document.fields](./catalog-document-fields.md) | This method returns a list of fields for inventory documents. | This method is deprecated since version **22.400.0**. It is recommended to use the method [catalog.document.getFields](./catalog-document-get-fields.md)||
|| [catalog.document.getFields](./catalog-document-get-fields.md) | This method returns a list of fields for inventory documents. | ||
|| [catalog.document.list](./catalog-document-list.md) | This method retrieves a list of documents. | ||
|| [catalog.document.mode.status](./catalog-document-mode-status.md) | This method retrieves information on whether inventory management is enabled. | ||
|| [catalog.document.unconfirm](./catalog-document-unconfirm.md) | This method cancels the processing of a document. | This method is deprecated since version **22.400.0**. It is recommended to use the method [catalog.document.cancel](./catalog-document-cancel.md).||
|| [catalog.document.update](./catalog-document-update.md) | This method updates an inventory document. | ||
|#

{% note info "Attention!" %}

Prior to version 22.400.0 of the catalog module, input parameters and results were passed as UPPER_CASE.

{% endnote %}