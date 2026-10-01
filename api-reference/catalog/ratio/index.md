# Measurement Unit Ratios in the Trade Catalog: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The measurement unit ratio shows how many units of a product are sold at a time. For example, if water is sold in packs of six bottles, its measurement unit is a bottle and its ratio is 6.

Ratios can only be read through REST. They are set in the product card in Bitrix24, in the "Unit ratio" column of the variations table. If the column is not visible, enable it in the list view settings.

{% note warning "" %}

The [catalog.product.add](../product/catalog-product-add.md) and [catalog.product.update](../product/catalog-product-update.md) methods do not retain the ratio. They skip the `measureRatio` field without an error and return a successful response, but the product ratio does not change.

{% endnote %}

> Quick navigation: [all methods](#all-methods)

## How to Start

1. Retrieve the product ID using the [catalog.product.list](../product/catalog-product-list.md) method
2. Retrieve the product ratios using the [catalog.ratio.list](./catalog-ratio-list.md) method with the filter `{"productId": <product ID>}`. Write the field name exactly as it appears in the response: the method skips the `PRODUCT_ID` field without an error and returns the ratios of all products
3. Select the record where `isDefault` equals `Y`. If there is no such record, Bitrix24 uses a ratio of 1 for the product. This happens, for example, with products created through REST
4. Request a known record by its `id` using the [catalog.ratio.get](./catalog-ratio-get.md) method

## Response Format

The methods return data in the following fields:

- [catalog.ratio.get](./catalog-ratio-get.md) — the ratio object in `result.ratio`
- [catalog.ratio.list](./catalog-ratio-list.md) — an array of objects in `result.ratios`, up to 50 records per call. The total number of records found is returned in `total`. If there is a next page, `next` contains the `start` value for it
- [catalog.ratio.getFields](./catalog-ratio-get-fields.md) — field descriptions in `result.ratio`

The ratio object has four fields:

- `id` — the identifier of the ratio record
- `productId` — the product identifier
- `ratio` — the ratio value, for example, `6`
- `isDefault` — `Y` if this is the default ratio of the product, otherwise `N`

## Relationship with Other Objects

The ratio is linked to a product and, through the product, to a measurement unit.

**Product.** The product identifier is stored in the `productId` field of the ratio. The [catalog.product.list](../product/catalog-product-list.md) method returns it, and the [catalog.product.get](../product/catalog-product-get.md) method returns the product data using that identifier.

**Measurement unit.** The ratio value is specified in the product's measurement unit. The identifier of this unit is stored in the `measure` field of the product. The list of units is returned by the [catalog.measure.list](../measure/catalog-measure-list.md) method of the [catalog.measure.*](../measure/index.md) group.

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the methods: a user with the "View Product Catalog" or "Manage Price Types" access permission

#|
|| **Method** | **Description** ||
|| [catalog.ratio.get](./catalog-ratio-get.md) | Returns the field values of the measurement unit ratio by identifier ||
|| [catalog.ratio.list](./catalog-ratio-list.md) | Returns a list of measurement unit ratios ||
|| [catalog.ratio.getFields](./catalog-ratio-get-fields.md) | Returns the available fields of the measurement unit ratio ||
|#
