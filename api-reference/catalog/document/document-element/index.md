# Methods for Working with Inventory Document Items

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

Methods for working with warehouses:

#|
|| **Method** | **Description** | **Note** ||
|| [catalog.document.element.add](./catalog-document-element-add.md) | This method adds an item to the inventory document. |  ||
|| [catalog.document.element.delete](./catalog-document-element-delete.md) | This method removes an item from the inventory document. |  ||
|| [catalog.document.element.fields](./catalog-document-element-fields.md) | This method retrieves the field values of the warehouse by ID. | This method is deprecated since version **22.400.0**. It is recommended to use the method [catalog.document.element.getFields](./catalog-document-element-get-fields.md). ||
|| [catalog.document.element.getFields](./catalog-document-element-get-fields.md) | This method returns a list of fields for items in the inventory document.  | ||
|| [catalog.document.element.list](./catalog-document-element-list.md) | This method retrieves a list of items in inventory documents.  | ||
|| [catalog.document.element.update](./catalog-document-element-update.md) | This method updates an item in the inventory document. |  ||
|#

{% note info "Attention!" %}

Before version 22.400.0 of the catalog module, input parameters and results were passed as UPPER_CASE.

{% endnote %}