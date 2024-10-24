# Price Rounding Rules in the Trade Catalog: Overview of Methods

The price rounding rule is a set of settings that determine how to modify the price of a product or service in the cart. They help automatically round prices, for example, to the nearest whole number or to a specific decimal place.

There are three types of price rounding:
- mathematical rounding
- rounding up: the price increases, which benefits the store
- rounding down: the price decreases, which benefits the customer

> Quick navigation: [all methods](#all-methods)

## Connection of Price Rounding Rules with Other Objects

**Price Types.** To create different types of prices: wholesale, retail, and partner, use the methods of the group [catalog.priceType.* ](../price-type/index.md). For each price type, you can set your own rounding rules. This will allow you to control how prices are displayed and applied.

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

#| 
|| **Method** | **Description** ||
|| [catalog.roundingRule.add](./catalog-rounding-rule-add.md) | Adds a price rounding rule ||
|| [catalog.roundingRule.update](./catalog-rounding-rule-update.md) | Modifies a price rounding rule ||
|| [catalog.roundingRule.get](./catalog-rounding-rule-get.md) | Returns information about a price rounding rule by its identifier ||
|| [catalog.roundingRule.list](./catalog-rounding-rule-list.md) | Returns a list of price rounding rules ||
|| [catalog.roundingRule.delete](./catalog-rounding-rule-delete.md) | Deletes a price rounding rule ||
|| [catalog.roundingRule.getFields](./catalog-rounding-rule-get-fields.md) | Returns the fields of a price rounding rule ||
|#