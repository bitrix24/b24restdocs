# Price Types in the Trade Catalog: Overview of Methods

A price type is an object in the Trade Catalog that allows management of various pricing categories for products and services. A single product can have multiple price types: wholesale, retail, partner.

One of the price types must be designated as the base type. The base price type cannot be deleted.

> Quick navigation: [all methods and events](#all-methods) 

## Relationship of Price Type with Other Objects

**Price.** When creating a price, you must specify its type. You can set and change the price using the methods [catalog.price.*](../price/index.md). 

**Price Rounding Rules.** Set rounding parameters for each price type using the methods [catalog.roundingRule.*](../rounding-rule/index.md).

**Translations of Price Type Names.** Specify the names of the price type for the languages used in Bitrix24. Use the methods [catalog.priceTypeLang.*](./price-type-lang/index.md).

## Overview of Methods and Events {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute methods: administrator

### Main

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [catalog.priceType.add](./catalog-price-type-add.md) | Adds a new price type ||
    || [catalog.priceType.update](./catalog-price-type-update.md) | Modifies the values of the price type fields ||
    || [catalog.priceType.get](./catalog-price-type-get.md) | Returns information about the price type by its identifier ||
    || [catalog.priceType.list](./catalog-price-type-list.md) | Returns a list of price types by filter ||
    || [catalog.priceType.delete](./catalog-price-type-delete.md) | Deletes a price type ||
    || [catalog.priceType.getFields](./catalog-price-type-get-fields.md) | Returns the fields of the price type ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [CATALOG.PRICE.TYPE.ON.ADD](./events/catalog-price-type-on-add.md) | When a price type is added ||
    || [CATALOG.PRICE.TYPE.ON.UPDATE](./events/catalog-price-type-on-update.md) | When a price type is updated ||
    || [CATALOG.PRICE.TYPE.ON.DELETE](./events/catalog-price-type-on-delete.md) | When a price type is deleted ||
    |#

{% endlist %}

### Translations of Price Type Names

#|
|| **Method** | **Description** ||
|| [catalog.priceTypeLang.add](./price-type-lang/catalog-price-type-lang-add.md) | Adds a translation for the price type name ||
|| [catalog.priceTypeLang.update](./price-type-lang/catalog-price-type-lang-update.md) | Updates the translation for the price type name ||
|| [catalog.priceTypeLang.get](./price-type-lang/catalog-price-type-lang-get.md) | Returns the values of the fields for the translation of the price type name ||
|| [catalog.priceTypeLang.list](./price-type-lang/catalog-price-type-lang-list.md) | Returns a list of translations of price type names by filter ||
|| [catalog.priceTypeLang.delete](./price-type-lang/catalog-price-type-lang-delete.md) | Deletes the translation for the price type name ||
|| [catalog.priceTypeLang.getLanguages](./price-type-lang/catalog-price-type-lang-get-languages.md) | Returns the available languages for translation ||
|| [catalog.priceTypeLang.getFields](./price-type-lang/catalog-price-type-lang-get-fields.md) | Returns the fields for the translation of the price type name ||
|#

### Binding Price Types to Customer Groups

#|
|| **Method** | **Description** ||
|| [catalog.priceTypeGroup.add](./price-type-group/catalog-price-type-group-add.md) | Adds a binding of the price type to a customer group ||
|| [catalog.priceTypeGroup.list](./price-type-group/catalog-price-type-group-list.md) | Returns a list of bindings of price types to customer groups ||
|| [catalog.priceTypeGroup.delete](./price-type-group/catalog-price-type-group-delete.md) | Deletes the binding of the price type to a customer group ||
|| [catalog.priceTypeGroup.getFields](./price-type-group/catalog-price-type-group-get-fields.md) | Returns the fields for the binding of price types to customer groups ||
|#