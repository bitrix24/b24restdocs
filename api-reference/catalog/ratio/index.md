# Measurement Unit Ratios in the Trade Catalog: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The measurement unit ratio indicates how a unit of product is accounted for. This allows for accurate quantity conversions during data exchanges.

The ratio object includes:

- `id` — the identifier of the measurement unit ratio
- `productId` — the identifier of the product to which the ratio pertains
- `ratio` — the numerical value of the measurement unit ratio
- `isDefault` — a flag indicating if this is the default ratio

> Quick navigation: [all methods](#all-methods)

## How to Work with Measurement Unit Ratios

1. Retrieve the structure and field types via [catalog.ratio.getFields](./catalog-ratio-get-fields.md).
2. Obtain a list of ratios through [catalog.ratio.list](./catalog-ratio-list.md). If you need the primary ratio for a product, select the record with `isDefault = Y`.
3. Get the data for a specific ratio by its `id` through [catalog.ratio.get](./catalog-ratio-get.md).

## Connection to Other Objects

**Product.** The ratio is linked to the product through the `productId` field. Product identifiers can be obtained using the methods [catalog.product.list](../product/catalog-product-list.md) and [catalog.product.get](../product/catalog-product-get.md).

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the methods: administrator

#| 
|| **Method** | **Description** ||
|| [catalog.ratio.get](./catalog-ratio-get.md) | Returns the field values of the measurement unit ratio by identifier ||
|| [catalog.ratio.list](./catalog-ratio-list.md) | Returns a list of measurement unit ratios ||
|| [catalog.ratio.getFields](./catalog-ratio-get-fields.md) | Returns the available fields of the measurement unit ratio ||
|#