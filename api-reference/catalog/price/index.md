# Overview of Methods and Events

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon

{% endnote %}

## Methods

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#| 
|| **Method** | **Description** ||
|| [catalog.price.add](./catalog-price-add.md) | This method adds a product price. ||
|| [catalog.price.delete](./catalog-price-delete.md) | This method deletes a product price. ||
|| [catalog.price.get](./catalog-price-get.md) | This method retrieves the price field values of a product by ID. ||
|| [catalog.price.getFields](./catalog-price-get-fields.md) | This method returns the fields of product prices. ||
|| [catalog.price.list](./catalog-price-list.md) | This method gets a list of product prices based on a filter. ||
|| [catalog.price.modify](./catalog-price-modify.md) | This method modifies elements of the product price collection. ||
|| [catalog.price.update](./catalog-price-update.md) | This method updates the fields of a product price. ||
|#

## Events

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can subscribe: any user

#| 
|| **Event** | **Triggered** ||
|| [CATALOG.PRICE.ON.ADD](../events/catalog-price-on-add.md)| When a price is added ||
|| [CATALOG.PRICE.ON.UPDATE](../events/catalog-price-on-update.md)| When a price is updated ||
|| [CATALOG.PRICE.ON.DELETE](../events/catalog-price-on-delete.md)| When a price is deleted ||
|#