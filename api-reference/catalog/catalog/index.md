# Trade Catalog: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The trade catalog is a tool for managing products and services in an online store. The catalog stores product information: name, description, price, images, and other parameters.

Cloud Bitrix24 has only one trade catalog, and you cannot create a new one. If you use products with variations, Bitrix24 will have two trade catalogs: a product catalog and a variation catalog.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Product catalog](https://helpdesk.bitrix24.com/open/25829591/)

## How to Start

1. Get the list of trade catalogs using [catalog.catalog.list](./catalog-catalog-list.md)
2. Check which catalog is used for variations using [catalog.catalog.isOffers](./catalog-catalog-is-offers.md)
3. Get the fields of the selected catalog using [catalog.catalog.get](./catalog-catalog-get.md)
4. Configure sections, properties, products, and VAT rates using related catalog methods

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
> Who can execute the methods: administrator

#|
|| **Method** | **Description** ||
|| [catalog.catalog.list](./catalog-catalog-list.md) | Returns a list of trade catalogs ||
|| [catalog.catalog.get](./catalog-catalog-get.md) | Returns the values of all trade catalog fields ||
|| [catalog.catalog.isOffers](./catalog-catalog-is-offers.md) | Checks whether a trade catalog is a variation catalog ||
|| [catalog.catalog.getFields](./catalog-catalog-get-fields.md) | Returns available trade catalog fields ||
|#
