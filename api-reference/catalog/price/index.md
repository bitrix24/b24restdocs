# Prices in the Trade Catalog: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A product can have one or more prices. For each price, the following is specified:

- currency: US dollar, euro
- type: retail, wholesale, partner

This allows for pricing to be configured based on the type of customer, region, or terms of cooperation.

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [Right to Change the Selling Price of a Product in the Catalog](https://helpdesk.bitrix24.com/open/16665980/)

## How to Start

1. Get the price type using [catalog.priceType.list](../price-type/catalog-price-type-list.md)
2. Get the identifier of a product or variation
3. Add a price using [catalog.price.add](./catalog-price-add.md)
4. Check the price using [catalog.price.get](./catalog-price-get.md) or [catalog.price.list](./catalog-price-list.md)
5. Track changes through [price events](./events/index.md)

## Prices in REST 3.0

In [REST 3.0](../../rest-v3.md), prices have methods for adding, updating, retrieving, and deleting, as well as the `catalog.price.field.list` and `catalog.price.field.get` methods for retrieving the field schema. Methods available in both API versions are marked `v 3.0` in the table.

Specifics of the new version methods:

- REST 3.0 has no `catalog.price.modify` method. To change a price and keep its identifier, use [catalog.price.update](./catalog-price-update-rest-v3.md)
- the product and the price type are set when the price is created. The `catalog.price.update` method does not change `productId` and `catalogGroupId`, even if you pass them in `fields`
- a base type price has no markup: `extraId` is reset to `0`. A non-existent `extraId` is also reset to `0`
- for a non-existent product, the [catalog.price.add](./catalog-price-add-rest-v3.md) method returns the `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION` access error, not a "not found" error
- empty `extraId`, `quantityFrom`, and `quantityTo` are returned as `0`

## Relationship with Other Objects

**Price Type.** Each price belongs to a specific price type. The price type can be set and changed using the methods [catalog.priceType.*](../price-type/index.md).

**Currency.** The price is specified in the selected currency. You can work with currencies through the methods [crm.currency.*](../../crm/currency/index.md).

**Markup.** Markup affects price calculation. You can get markup using [catalog.extra.get](../extra/catalog-extra-get.md). Markup is available only in Bitrix24 with the advanced price management option enabled.

**Product.** The price is always tied to a product. You can create and edit a product using the following groups of methods:

- [catalog.product.*](../product/index.md) — for simple products
- [catalog.product.service.*](../product/service/index.md) — for services
- [catalog.product.sku.*](../product/sku/index.md) — for parent products with variations
- [catalog.product.offer.*](../product/offer/index.md) — for product variations

{% note tip "User Documentation" %}

- [How to add and configure currencies in CRM](https://helpdesk.bitrix24.com/open/8967215/)
- [How to create a product in the catalog](https://helpdesk.bitrix24.com/open/18532198/)
- [Services in CRM](https://helpdesk.bitrix24.com/open/16680094/)
- [How to work with product variations](https://helpdesk.bitrix24.com/open/18839102/)

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
    || [catalog.price.add](./catalog-price-add-rest-v3.md) | Adds a product price v 3.0 ||
    || [catalog.price.update](./catalog-price-update.md) | Updates a product price ||
    || [catalog.price.update](./catalog-price-update-rest-v3.md) | Updates a product price v 3.0 ||
    || [catalog.price.get](./catalog-price-get.md) | Returns the values of the product price fields by identifier ||
    || [catalog.price.get](./catalog-price-get-rest-v3.md) | Returns the values of the product price fields by identifier v 3.0 ||
    || [catalog.price.list](./catalog-price-list.md) | Returns a list of product prices by filter ||
    || [catalog.price.list](./catalog-price-list-rest-v3.md) | Returns a list of product prices by filter v 3.0 ||
    || [catalog.price.delete](./catalog-price-delete.md) | Deletes a product price ||
    || [catalog.price.delete](./catalog-price-delete-rest-v3.md) | Deletes a product price v 3.0 ||
    || [catalog.price.modify](./catalog-price-modify.md) | Updates the collection of product prices ||
    || [catalog.price.getFields](./catalog-price-get-fields.md) | Returns the fields of product prices ||
    || [catalog.price.field.list](./catalog-price-field-list.md) | Returns a list of price fields v 3.0 ||
    || [catalog.price.field.get](./catalog-price-field-get.md) | Returns the description of a price field ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [CATALOG.PRICE.ON.ADD](./events/catalog-price-on-add.md) | When a price is added ||
    || [CATALOG.PRICE.ON.UPDATE](./events/catalog-price-on-update.md) | When a price is updated ||
    || [CATALOG.PRICE.ON.DELETE](./events/catalog-price-on-delete.md) | When a price is deleted ||
    |#

{% endlist %}
