# Event Overview

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can subscribe: any user

When an outgoing webhook is triggered, the following parameters are added to the webhook URL at the moment the registered event occurs:

```php
'FIELDS' => [
    'ID' => $id // object id
],
```

## Units of Measurement

#|
|| **Event** | **Triggered** ||
|| [CATALOG.MEASURE.ON.ADD](catalog-measure-on-add.md) | When a unit of measurement is added ||
|| [CATALOG.MEASURE.ON.UPDATE](catalog-measure-on-update.md)| When a unit of measurement is updated ||
|| [CATALOG.MEASURE.ON.DELETE](catalog-measure-on-delete.md)| When a unit of measurement is deleted ||
|#

## Price

#|
|| **Event** | **Triggered** ||
|| [CATALOG.PRICE.ON.ADD](catalog-price-on-add.md)| When a price is added ||
|| [CATALOG.PRICE.ON.UPDATE](catalog-price-on-update.md)| When a price is updated ||
|| [CATALOG.PRICE.ON.DELETE](catalog-price-on-delete.md)| When a price is deleted ||
|#

## Price Type

#|
|| **Event** | **Triggered** ||
|| [CATALOG.PRICE.TYPE.ON.ADD](catalog-price-type-on-add.md)| When a price type is added ||
|| [CATALOG.PRICE.TYPE.ON.UPDATE](catalog-price-type-on-update.md)| When a price type is updated ||
|| [CATALOG.PRICE.TYPE.ON.DELETE](catalog-price-type-on-delete.md)| When a price type is deleted ||
|#

## Product

#|
|| **Event** | **Triggered** ||
|| [CATALOG.PRODUCT.ON.ADD](catalog-product-on-add.md)| When a product is added ||
|| [CATALOG.PRODUCT.ON.UPDATE](catalog-product-on-update.md)| When a product is updated ||
|| [CATALOG.PRODUCT.ON.DELETE](catalog-product-on-delete.md)| When a product is deleted ||
|#

## Price Rounding Rule

#|
|| **Event** | **Triggered** ||
|| [CATALOG.ROUNDING.ON.ADD](catalog-rounding-on-add.md)| When a price rounding rule is added ||
|| [CATALOG.ROUNDING.ON.UPDATE](catalog-rounding-on-update.md)| When a price rounding rule is updated ||
|| [CATALOG.ROUNDING.ON.DELETE](catalog-rounding-on-delete.md)| When a price rounding rule is deleted ||
|#