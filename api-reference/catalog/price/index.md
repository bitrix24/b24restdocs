# Prices in the Trade Catalog: Overview of Methods

A product can have one or more prices. For each price, the following is specified:

- currency: US dollar, euro,
- type: retail, wholesale, partner.

This allows for pricing to be configured based on the type of customer, region, or terms of cooperation.

> Quick navigation: [all methods and events](#all-methods) 
>
> User documentation: [Right to change the selling price of a product in the catalog](https://helpdesk.bitrix24.com/open/16665980/)

## Connection of Prices with Other Objects

**Price Type.** Each price belongs to a specific price type. The price type can be set and changed using the methods [catalog.priceType.*](../price-type/index.md).

**Currency.** The price is specified in the selected currency. You can work with currencies through the methods [crm.currency.*](../../crm/currency/index.md).

**Markup.** It affects the formation of the price. The markup can be obtained using the method [catalog.extra.get](../extra/catalog-extra-get.md). The use of markup is available only in Bitrix24 with the active option for advanced price management, which is now deprecated.

**Product.** The price is always tied to a product. You can create and edit a product using the following groups of methods:

- [catalog.product.*](../product/index.md) — for simple products

- [catalog.product.service.*](../product/service/index.md) — for services

- [catalog.product.sku.*](../product/sku/index.md) — for parent products with variations

- [catalog.product.offer.*](../product/offer/index.md) — for product variations

{% note tip "User Documentation" %}

- [Currencies in CRM](https://helpdesk.bitrix24.com/open/8967215/)

- [How to create a new product in the catalog](https://helpdesk.bitrix24.com/open/18532198/)

- [Services in CRM](https://helpdesk.bitrix24.com/open/16680094/)

- [Working with product variations](https://helpdesk.bitrix24.com/open/18839102/)

{% endnote %}

## Overview of Methods and Events {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [catalog.price.add](./catalog-price-add.md) | Adds a product price ||
    || [catalog.price.update](./catalog-price-update.md) | Updates a product price ||
    || [catalog.price.modify](./catalog-price-modify.md) | Updates the collection of product prices ||    
    || [catalog.price.getFields](./catalog-price-get-fields.md) | Returns the fields of product prices ||
    || [catalog.price.list](./catalog-price-list.md) | Returns a list of product prices by filter ||
    || [catalog.price.get](./catalog-price-get.md) | Returns the values of the product price fields by identifier ||
    || [catalog.price.delete](./catalog-price-delete.md) | Deletes a product price ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [CATALOG.PRICE.ON.ADD](./events/catalog-price-on-add.md)| When a price is added ||
    || [CATALOG.PRICE.ON.UPDATE](./events/catalog-price-on-update.md)| When a price is updated ||
    || [CATALOG.PRICE.ON.DELETE](./events/catalog-price-on-delete.md)| When a price is deleted ||
    |#

{% endlist %}