# Properties of the Shopping Cart in an Online Store: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Cart properties are characteristics of a specific [cart item](../basket-item/index.md) in an order: size, color, article number. For example, a customer orders a T-shirt, and the application retains the "Size: M" property for this item. The same T-shirt in another order may be size L.

A cart item property is a separate object, not a product property in the catalog: it applies to only one item of an order. Bitrix24 stores its name and value as text, even if it copied them from the characteristics of a product variation.

> Quick navigation: [all methods](#all-methods)

## Connection of Cart Properties with Other Objects

**Cart item and order.** Each property belongs to one cart item, and the item ID is passed in `basketId`. The item must belong to an order: its `orderId` field is filled in the [sale.basketitem.list](../basket-item/sale-basket-item-list.md) response. For an item without an order, the [sale.basketproperties.add](./sale-basket-properties-add.md) method returns the `MAIN_CONTROLLER_22001` error.

**Catalog products.** Properties of products and variations in the catalog are created and configured by the [catalog.productProperty.*](../../catalog/product-property/index.md) methods. Working with them in the interface is described in the article [Create and Configure Product Properties in CRM](https://helpdesk.bitrix24.com/open/25882983/).

## How a Property Is Structured

#|
|| **Field** | **What it stores** | **Example** ||
|| `id` | Property ID. It is returned by the [sale.basketproperties.add](./sale-basket-properties-add.md) method | `1009` ||
|| `basketId` | ID of the cart item the property belongs to | `1247` ||
|| `name` | Property name | `Size` ||
|| `value` | Property value | `M` ||
|| `code` | Symbolic code. It is used to search for properties with the [sale.basketproperties.list](./sale-basket-properties-list.md) method | `MYAPP_SIZE` ||
|| `sort` | Number used to sort the item's properties. Default is `100` | `10` ||
|| `xmlId` | External property code. If it is not passed, Bitrix24 generates the code itself | `bx_6ab4f0d92abaa` ||
|#

The [sale.basketproperties.add](./sale-basket-properties-add.md), [sale.basketproperties.get](./sale-basket-properties-get.md), and [sale.basketproperties.update](./sale-basket-properties-update.md) methods return the property in `result.basketProperty`, and the [sale.basketproperties.delete](./sale-basket-properties-delete.md) method returns `true` in `result`. The [sale.basketproperties.list](./sale-basket-properties-list.md) method returns up to 50 properties per call in `result.basketProperties` and the total number found in `total`. Request the next page with the `start` parameter. Without the `order` parameter, the list is sorted by `id` in ascending order, not by `sort`.

## How to Get Started

1. Retrieve the order items using the [sale.basketitem.list](../basket-item/sale-basket-item-list.md) method with a filter by `orderId` and retain the `id` of the item you need.
2. Add a property using the [sale.basketproperties.add](./sale-basket-properties-add.md) method: pass `basketId`, `name`, `value`, and `code`.
3. Check the item's properties using the [sale.basketproperties.list](./sale-basket-properties-list.md) method with a filter by `basketId`. The [sale.basketproperties.get](./sale-basket-properties-get.md) method returns a single property by its ID.
4. To change a property, call [sale.basketproperties.update](./sale-basket-properties-update.md) and pass `name`, `value`, and `code` together, even if you change only the value.
5. Delete a property you no longer need using the [sale.basketproperties.delete](./sale-basket-properties-delete.md) method.

## What to Consider

- The `name`, `value`, `code`, and `xmlId` fields store up to 255 characters. Bitrix24 truncates a longer value without an error.
- Bitrix24 does not check `code` for uniqueness: calling [sale.basketproperties.add](./sale-basket-properties-add.md) again with the same `code` creates a second property for the item. To avoid a duplicate, first search for the property with the [sale.basketproperties.list](./sale-basket-properties-list.md) method by `basketId` and `code`.
- The [sale.basketproperties.list](./sale-basket-properties-list.md) response may contain properties that the application did not create: the internal properties `CATALOG.XML_ID` and `PRODUCT.XML_ID` of catalog items or characteristics of a product variation. To tell your own properties apart, start `code` with the application's prefix, for example `MYAPP_SIZE`. To update and delete properties, retain the `id` from the [sale.basketproperties.add](./sale-basket-properties-add.md) response.

{% note warning "" %}

A property cannot be moved to another item. The [sale.basketproperties.update](./sale-basket-properties-update.md) method accepts a new `basketId` without an error, but the property stays with the previous item. To make the property appear for the right item, delete it and create it again.

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the methods: depending on the method — a store manager can retrieve properties, any user can retrieve the field descriptions, and an administrator can add, update, and delete properties

#| 
|| **Method** | **Description** ||
|| [sale.basketproperties.add](./sale-basket-properties-add.md) | Adds a property to the cart item ||
|| [sale.basketproperties.update](./sale-basket-properties-update.md) | Updates the fields of the cart item property ||
|| [sale.basketproperties.get](./sale-basket-properties-get.md) | Returns the cart item property by its identifier ||
|| [sale.basketproperties.list](./sale-basket-properties-list.md) | Returns a list of properties of cart items ||
|| [sale.basketproperties.delete](./sale-basket-properties-delete.md) | Deletes a property of the cart item ||
|| [sale.basketproperties.getFields](./sale-basket-properties-get-fields.md) | Returns the description of the cart item property fields ||
|#