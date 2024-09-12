# Methods for Working with Inventory Accounting

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can perform the method: any user

Methods for working with inventory accounting:

#|
|| **Method** | **Description** | **Note** ||
|| [catalog.document.add](./catalog-document-add.md) | This method adds an inventory accounting document. | ||
|| [catalog.document.cancel](./catalog-document-cancel.md) | This method cancels the processing of an inventory accounting document by ID. | ||
|| [catalog.document.cancelList](./catalog-document-cancel-list.md) | This method cancels the processing of a group of inventory accounting documents. | ||
|| [catalog.document.conduct](./catalog-document-conduct.md) | This method is used to process an inventory accounting document. | ||
|| [catalog.document.conductList](./catalog-document-conduct-list.md) | This method is used for bulk processing of inventory accounting documents. | ||
|| [catalog.document.confirm](./catalog-document-confirm.md) | This method is used to process a document. | This method is deprecated since version **22.400.0**. It is recommended to use the method [catalog.document.conduct](./catalog-document-conduct.md).||
|| [catalog.document.delete](./catalog-document-delete.md) | This method deletes an inventory accounting document. | ||
|| [catalog.document.deleteList](./catalog-document-delete-list.md) | This method is used for bulk deletion of inventory accounting documents. | ||
|| [catalog.document.fields](./catalog-document-fields.md) | This method returns a list of fields for inventory accounting documents. | This method is deprecated since version **22.400.0**. It is recommended to use the method [catalog.document.getFields](./catalog-document-get-fields.md)||
|| [catalog.document.getFields](./catalog-document-get-fields.md) | This method returns a list of fields for inventory accounting documents. | ||
|| [catalog.document.list](./catalog-document-list.md) | This method retrieves a list of documents. | ||
|| [catalog.document.mode.status](./catalog-document-mode-status.md) | This method retrieves information on whether inventory accounting is enabled. | ||
|| [catalog.document.unconfirm](./catalog-document-unconfirm.md) | This method cancels the processing of a document. | This method is deprecated since version **22.400.0**. It is recommended to use the method [catalog.document.cancel](./catalog-document-cancel.md).||
|| [catalog.document.update](./catalog-document-update.md) | This method updates an inventory accounting document. | ||
|#

{% note info "Attention!" %}

Before version 22.400.0 of the catalog module, input parameters and results were passed as UPPER_CASE.

{% endnote %}