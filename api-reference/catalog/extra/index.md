# Markups in the Trade Catalog: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Markup is used to calculate the price of a product. The markup object includes the identifier `id`, the name `name`, and the percentage value `percentage`.

> Quick navigation: [all methods](#all-methods)

## How to Work with Markups

1. Retrieve the list of markups using the [catalog.extra.list](./catalog-extra-list.md) method.
2. Get the required markup by its identifier using the [catalog.extra.get](./catalog-extra-get.md) method.
3. If necessary, check the structure of the fields using the [catalog.extra.getFields](./catalog-extra-get-fields.md) method.

## Markups in REST 3.0

In [REST 3.0](../../rest-v3.md), markups are read-only: there are methods for retrieving a markup and a list of markups, as well as the `catalog.extra.field.list` and `catalog.extra.field.get` methods for retrieving the field schema. Methods available in both API versions are marked `v 3.0` in the table.

## Relation to Other Objects

**Product Price.** The markup is linked to the price through the `extraId` field in the price object. You can retrieve prices and their fields using the [catalog.price.list](../price/catalog-price-list.md), [catalog.price.get](../price/catalog-price-get.md), and [catalog.price.getFields](../price/catalog-price-get-fields.md) methods.

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the methods:
> - administrator — for methods of the previous API version
> - a user with the "View product catalog", "Edit selling price", or "Manage mark-ups" access permission — for REST 3.0 methods

#| 
|| **Method** | **Description** ||
|| [catalog.extra.get](./catalog-extra-get.md) | Returns information about the markup by identifier ||
|| [catalog.extra.get](./catalog-extra-get-rest-v3.md) | Returns information about the markup by identifier v 3.0 ||
|| [catalog.extra.list](./catalog-extra-list.md) | Returns a list of markups based on a filter ||
|| [catalog.extra.list](./catalog-extra-list-rest-v3.md) | Returns a list of markups based on a filter v 3.0 ||
|| [catalog.extra.getFields](./catalog-extra-get-fields.md) | Returns fields of the markup ||
|| [catalog.extra.field.list](./catalog-extra-field-list.md) | Returns a list of markup fields v 3.0 ||
|| [catalog.extra.field.get](./catalog-extra-field-get.md) | Returns the description of a markup field ||
|#