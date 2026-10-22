# Trade Catalog: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The trade catalog is a tool for managing products and services in an online store. The catalog stores product information: name, description, price, images, and other parameters.

By default, cloud Bitrix24 has one trade catalog — the CRM product catalog. If you use products with variations, Bitrix24 will have two trade catalogs: a product catalog and a variation catalog. In REST 3.0, you can also turn another information block into a trade catalog using the [catalog.catalog.add](./catalog-catalog-add.md) method.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Product catalog](https://helpdesk.bitrix24.com/open/25829591/)

## How to Start

1. Get the list of trade catalogs using [catalog.catalog.list](./catalog-catalog-list.md)
2. Check which catalog is used for variations using [catalog.catalog.isOffers](./catalog-catalog-is-offers.md)
3. Get the fields of the selected catalog using [catalog.catalog.get](./catalog-catalog-get.md)
4. Configure sections, properties, products, and VAT rates using related catalog methods

## Trade Catalog in REST 3.0

In [REST 3.0](../../rest-v3.md), the trade catalog has methods for adding, updating, and deleting, as well as the `catalog.catalog.field.list` and `catalog.catalog.field.get` methods for retrieving the field schema. Methods available in both API versions are marked `v 3.0` in the table.

Specifics of the new version methods:

- the [catalog.catalog.add](./catalog-catalog-add.md) method turns an existing information block that is not yet a catalog into a trade catalog. The catalog identifier matches the information block identifier
- the [catalog.catalog.delete](./catalog-catalog-delete.md) method removes the trade catalog flag from the information block; the information block itself and its elements remain. A variation catalog cannot be deleted
- the method for checking a variation catalog is named [catalog.catalog.isoffers](./catalog-catalog-is-offers-rest-v3.md) and is called in lowercase only

## Relationship with Other Objects

**VAT Rates.** Set a VAT rate for all catalog products using the [catalog.vat.*](../vat/index.md) methods.

**Catalog Sections.** Create the trade catalog structure by adding sections and subsections. Use the [catalog.section.*](../section/index.md) method group.

**Product and Variation Properties.** Products and variations have properties that distinguish them from each other, such as color or size. These properties help customers search for and choose products on the site. Create properties using the [catalog.productProperty.*](../product-property/index.md) methods.

**Products.** Create and edit products using the following method groups:
- [catalog.product.*](../product/index.md) for simple products
- [catalog.product.service.*](../product/service/index.md) for services
- [catalog.product.sku.*](../product/sku/index.md) for parent products of products with variations
- [catalog.product.offer.*](../product/offer/index.md) for product variations

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the methods: administrator. The `catalog.catalog.field.list` and `catalog.catalog.field.get` methods — any user

#|
|| **Method** | **Description** ||
|| [catalog.catalog.add](./catalog-catalog-add.md) | Turns an existing information block into a trade catalog ||
|| [catalog.catalog.update](./catalog-catalog-update.md) | Updates the trade catalog settings ||
|| [catalog.catalog.list](./catalog-catalog-list.md) | Returns a list of trade catalogs ||
|| [catalog.catalog.list](./catalog-catalog-list-rest-v3.md) | Returns a list of trade catalogs v 3.0 ||
|| [catalog.catalog.get](./catalog-catalog-get.md) | Returns the values of all trade catalog fields ||
|| [catalog.catalog.get](./catalog-catalog-get-rest-v3.md) | Returns the values of all trade catalog fields v 3.0 ||
|| [catalog.catalog.delete](./catalog-catalog-delete.md) | Removes the trade catalog flag from an information block ||
|| [catalog.catalog.isOffers](./catalog-catalog-is-offers.md) | Checks whether a trade catalog is a variation catalog ||
|| [catalog.catalog.isoffers](./catalog-catalog-is-offers-rest-v3.md) | Checks whether a trade catalog is a variation catalog v 3.0 ||
|| [catalog.catalog.getFields](./catalog-catalog-get-fields.md) | Returns available trade catalog fields ||
|| [catalog.catalog.field.list](./catalog-catalog-field-list.md) | Returns a list of trade catalog fields v 3.0 ||
|| [catalog.catalog.field.get](./catalog-catalog-field-get.md) | Returns the description of a trade catalog field ||
|#
