# Events of the Trade Catalog

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

For all events, the incoming parameter is the identifier (ID) of the entity that triggered the event.

#|
|| **Event** | **Triggered By** ||
|| [CATALOG.MEASURE.ON.ADD](catalog-measure-on-add.md) | When a unit of measurement is added. ||
|| [CATALOG.MEASURE.ON.DELETE](catalog-measure-on-delete.md)| When a unit of measurement is deleted. ||
|| [CATALOG.MEASURE.ON.UPDATE](catalog-measure-on-update.md)| When a unit of measurement is updated. ||
|| [CATALOG.PRICE.ON.ADD](catalog-price-on-add.md)| When a price is added. ||
|| [CATALOG.PRICE.ON.DELETE](catalog-price-on-delete.md)| When a price is deleted. ||
|| [CATALOG.PRICE.ON.UPDATE](catalog-price-on-update.md)| When a price is updated. ||
|| [CATALOG.PRICE.TYPE.ON.ADD](catalog-price-type-on-add.md)| When a price type is added. ||
|| [CATALOG.PRICE.TYPE.ON.DELETE](catalog-price-type-on-delete.md)| When a price type is deleted. ||
|| [CATALOG.PRICE.TYPE.ON.UPDATE](catalog-price-type-on-update.md)| When a price type is updated. ||
|| [CATALOG.PRODUCT.ON.ADD](catalog-product-on-add.md)| When a product is added. ||
|| [CATALOG.PRODUCT.ON.DELETE](catalog-product-on-delete.md)| When a product is deleted. ||
|| [CATALOG.PRODUCT.ON.UPDATE](catalog-product-on-update.md)| When a product is updated. ||
|| [CATALOG.ROUNDING.ON.ADD](catalog-rounding-on-add.md)| When a price rounding rule is added. ||
|| [CATALOG.ROUNDING.ON.DELETE](catalog-rounding-on-delete.md)| When a price rounding rule is deleted. ||
|| [CATALOG.ROUNDING.ON.UPDATE](catalog-rounding-on-update.md)| When a price rounding rule is updated. ||
|| [CATALOG.STORE.ON.ADD](catalog-store-on-add.md)| When a warehouse is added. ||
|| [CATALOG.STORE.ON.DELETE](catalog-store-on-delete.md)| When a warehouse is deleted. ||
|| [CATALOG.STORE.ON.UPDATE](catalog-store-on-update.md)| When a warehouse is updated. ||
|#

As a result of the outgoing webhook call at the moment the registered event occurs, the link in the webhook is enriched with the following parameters:

```php
'FIELDS' => [
    'ID' => $id // ID of the entity
],
```