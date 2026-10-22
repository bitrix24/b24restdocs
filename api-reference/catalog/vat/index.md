# VAT Rates in the Trade Catalog: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The VAT rate can be set for the entire catalog or for a specific product. The product's VAT rate takes precedence. The catalog rate is applied if the product rate is not specified.

If a product or catalog has a VAT rate, general CRM taxes do not apply to them.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Taxes in CRM](https://helpdesk.bitrix24.com/open/17230492/)

## How to Start

1. Create a VAT rate using [catalog.vat.add](./catalog-vat-add.md)
2. Get the VAT rate identifier using [catalog.vat.get](./catalog-vat-get.md) or [catalog.vat.list](./catalog-vat-list.md)
3. Specify the VAT rate in trade catalog or product settings
4. Change the rate using [catalog.vat.update](./catalog-vat-update.md), if needed

## VAT Rates in REST 3.0

In [REST 3.0](../../rest-v3.md), VAT rates have methods for adding, updating, retrieving, and deleting, as well as the `catalog.vat.field.list` and `catalog.vat.field.get` methods for retrieving the field schema. Methods available in both API versions are marked `v 3.0` in the table.

Specifics of the new version methods:

- the `active` field accepts and returns `true` or `false`; the values `Y` and `N` are not supported
- when creating a rate using the [catalog.vat.add](./catalog-vat-add-rest-v3.md) method, `name` and `rate` are required. A rate without VAT with `rate` set to `null` cannot be created

## Relationship with Other Objects

**Trade Catalog.** Find out what VAT rate is set for the trade catalog using the method [catalog.catalog.get](../catalog/catalog-catalog-get.md).

**Products.** Specify the product's VAT using the following groups of methods:

- [catalog.product.*](../product/index.md) — for simple products
- [catalog.product.service.*](../product/service/index.md) — for services
- [catalog.product.sku.*](../product/sku/index.md) — for parent products of products with variations
- [catalog.product.offer.*](../product/offer/index.md) — for variations of products

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the methods:
> - administrator — for methods of the previous API version
> - a user with the "View product catalog" or "Manage tax rates" access permission — for retrieving rates in REST 3.0. Adding, updating, and deleting require the "Manage tax rates" access permission

#|
|| **Method** | **Description** ||
|| [catalog.vat.add](./catalog-vat-add.md) | Adds a VAT rate ||
|| [catalog.vat.add](./catalog-vat-add-rest-v3.md) | Adds a VAT rate v 3.0 ||
|| [catalog.vat.update](./catalog-vat-update.md) | Modifies a VAT rate ||
|| [catalog.vat.update](./catalog-vat-update-rest-v3.md) | Modifies a VAT rate v 3.0 ||
|| [catalog.vat.get](./catalog-vat-get.md) | Returns the VAT rate field values by identifier ||
|| [catalog.vat.get](./catalog-vat-get-rest-v3.md) | Returns the VAT rate field values by identifier v 3.0 ||
|| [catalog.vat.list](./catalog-vat-list.md) | Returns a list of VAT rates by filter ||
|| [catalog.vat.list](./catalog-vat-list-rest-v3.md) | Returns a list of VAT rates by filter v 3.0 ||
|| [catalog.vat.delete](./catalog-vat-delete.md) | Deletes a VAT rate ||
|| [catalog.vat.delete](./catalog-vat-delete-rest-v3.md) | Deletes a VAT rate v 3.0 ||
|| [catalog.vat.getFields](./catalog-vat-get-fields.md) | Returns the fields of the VAT rate ||
|| [catalog.vat.field.list](./catalog-vat-field-list.md) | Returns a list of VAT rate fields v 3.0 ||
|| [catalog.vat.field.get](./catalog-vat-field-get.md) | Returns the description of a VAT rate field ||
|#
