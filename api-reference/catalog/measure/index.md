# Measurement Units in the Trade Catalog: Overview of Methods

Every product has a measurement unit: weight, quantity, volume, and so on. Bitrix24 offers popular options, but you can add your own.

To set the primary measurement unit, specify the value `Y` in the `isDefault` parameter. Then, when creating a new product, it will be automatically selected in the detail form.

> Quick navigation: [all methods and events](#all-methods)
> 
> User documentation: 
> - [Units of measurement](https://helpdesk.bitrix24.com/open/7921829/)
> - [Units of measurement in services](https://helpdesk.bitrix24.com/open/17366738/)

## Measurement Unit Ratio

If you sell products by fractions or in packs, specify the measurement unit ratio in the product detail form. Examples:
- for fabric, set the ratio to 0.1 so that customers can order in increments of 0.1 meters, for example, 3.6 meters,
- for beverages in packs of 6 bottles, use a ratio of 6: 1 unit equals 6 bottles, 3 units equals 18 bottles.

You can view measurement unit ratios using the methods [catalog.ratio.*](../ratio/index.md).

## Connection of Measurement Units with Other Objects

**Products.** Create and edit products using the following groups of methods:
- [catalog.product.*](../product/index.md) — for simple products
- [catalog.product.service.*](../product/service/index.md) — for services
- [catalog.product.sku.*](../product/sku/index.md) — for parent products of products with variations
- [catalog.product.offer.*](../product/offer/index.md) — for variations of products

## Overview of Methods and Events {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute methods: administrator

{% list tabs %}

- Methods
  
    #|
    || **Method** | **Description** ||
    || [catalog.measure.add](./catalog-measure-add.md) | Adds a measurement unit ||
    || [catalog.measure.update](./catalog-measure-update.md) | Updates a measurement unit ||
    || [catalog.measure.get](./catalog-measure-get.md) | Returns information about a measurement unit by its identifier ||
    || [catalog.measure.list](./catalog-measure-list.md) | Returns a list of measurement units ||
    || [catalog.measure.delete](./catalog-measure-delete.md) | Deletes a measurement unit ||
    || [catalog.measure.getFields](./catalog-measure-get-fields.md) | Returns available fields of measurement units ||
    |#

- Events 

    #|
    || **Event** | **Triggered** ||
    || [CATALOG.MEASURE.ON.ADD](./events/catalog-measure-on-add.md) | When a measurement unit is added ||
    || [CATALOG.MEASURE.ON.UPDATE](./events/catalog-measure-on-update.md)| When a measurement unit is updated ||
    || [CATALOG.MEASURE.ON.DELETE](./events/catalog-measure-on-delete.md)| When a measurement unit is deleted ||
    |#

{% endlist %}