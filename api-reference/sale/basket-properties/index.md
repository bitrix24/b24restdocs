# Properties of the Shopping Cart in an Online Store: Overview of Methods

The properties of the cart are characteristics of the [cart items](../basket-item/index.md): size, color, article number, manufacturer.

> Quick navigation: [all methods](#all-methods)

## Connection of Cart Properties with Other Objects

**Cart.** Specify the cart item to which the property is linked. The list of cart items can be obtained using the [sale.basketitem.list](../basket-item/sale-basket-item-list.md) method.

## Overview of Methods {#all-methods}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the methods: depending on the method

#| 
|| **Method** | **Description** ||
|| [sale.basketproperties.add](./sale-basket-properties-add.md) | Adds a property to the cart item ||
|| [sale.basketproperties.update](./sale-basket-properties-update.md) | Updates the fields of the cart item property ||
|| [sale.basketproperties.get](./sale-basket-properties-get.md) | Returns the value of the cart item property by its identifier ||
|| [sale.basketproperties.list](./sale-basket-properties-list.md) | Returns a list of properties of cart items ||
|| [sale.basketproperties.delete](./sale-basket-properties-delete.md) | Deletes a property of the cart item ||
|| [sale.basketproperties.getFields](./sale-basket-properties-get-fields.md) | Returns a list of fields of the cart item property ||
|#