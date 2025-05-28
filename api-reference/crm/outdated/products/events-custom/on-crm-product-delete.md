# Event onCrmProductDelete

{% note warning "Event development has been halted" %}

The event `onCrmProductDelete` is still operational, but there is a more relevant alternative [CATALOG.PRODUCT.ON.DELETE](../../../../catalog/product/events/catalog-product-on-delete.md).

{% endnote %}

The event is triggered when a product is deleted.

## Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **FIELDS** | The array contains the ID field with the value of the deleted product's identifier ||
|#