# Markups in the Trade Catalog: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Markup is used to calculate the price of a product. The markup object includes the identifier `id`, the name `name`, and the percentage value `percentage`.

> Quick navigation: [all methods](#all-methods)

## How to Work with Markups

1. Retrieve the list of markups using the [catalog.extra.list](./catalog-extra-list.md) method.
2. Get the required markup by its identifier using the [catalog.extra.get](./catalog-extra-get.md) method.
3. If necessary, check the structure of the fields using the [catalog.extra.getFields](./catalog-extra-get-fields.md) method.

## Relation to Other Objects

**Product Price.** The markup is linked to the price through the `extraId` field in the price object. You can retrieve prices and their fields using the [catalog.price.list](../price/catalog-price-list.md), [catalog.price.get](../price/catalog-price-get.md), and [catalog.price.getFields](../price/catalog-price-get-fields.md) methods.

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

#| 
|| **Method** | **Description** ||
|| [catalog.extra.get](./catalog-extra-get.md) | Returns information about the markup by identifier ||
|| [catalog.extra.list](./catalog-extra-list.md) | Returns a list of markups based on a filter ||
|| [catalog.extra.getFields](./catalog-extra-get-fields.md) | Returns fields of the markup ||
|#