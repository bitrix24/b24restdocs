# Price Rounding Rules in the Trade Catalog: Overview of Methods

The price rounding rule is a set of settings that define how to modify the price of a product or service in the cart. They help automatically round prices, for example, to the nearest whole number or to a specific decimal place.

There are three types of price rounding:
- mathematical rounding
- rounding up: the price increases, which benefits the store
- rounding down: the price decreases, which benefits the customer

> Quick navigation: [all methods and events](#all-methods) 

## Connection of Price Rounding Rules with Other Objects

**Price Types.** To create different types of prices: wholesale, retail, and partner, use the methods of the group [catalog.priceType.* ](../price-type/index.md). For each price type, you can set your own rounding rules. This will allow you to control how prices will be displayed and applied.

## Overview of Methods and Events {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [catalog.roundingRule.add](./catalog-rounding-rule-add.md) | Adds a price rounding rule ||
    || [catalog.roundingRule.update](./catalog-rounding-rule-update.md) | Modifies a price rounding rule ||
    || [catalog.roundingRule.get](./catalog-rounding-rule-get.md) | Returns information about a price rounding rule by its identifier ||
    || [catalog.roundingRule.list](./catalog-rounding-rule-list.md) | Returns a list of price rounding rules ||
    || [catalog.roundingRule.delete](./catalog-rounding-rule-delete.md) | Deletes a price rounding rule ||
    || [catalog.roundingRule.getFields](./catalog-rounding-rule-get-fields.md) | Returns the fields of a price rounding rule ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [CATALOG.ROUNDING.ON.ADD](./events/catalog-rounding-on-add.md) | When a price rounding rule is added ||
    || [CATALOG.ROUNDING.ON.UPDATE](./events/catalog-rounding-on-update.md) | When a price rounding rule is updated ||
    || [CATALOG.ROUNDING.ON.DELETE](./events/catalog-rounding-on-delete.md) | When a price rounding rule is deleted ||
    |#

{% endlist %}