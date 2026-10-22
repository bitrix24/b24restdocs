# Sections in the Trade Catalog: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Sections are used to organize products and services in the catalog.

A product or service can be linked to multiple sections. For example, the product "Bandana" can appear in the sections "For Sports," "Accessories," and "Headwear." One of the sections must be marked as primary to prevent search engines from considering the product "Bandana" as duplicate content.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [How to create and configure a product catalog](https://helpdesk.bitrix24.com/open/21121728/)

## How to Start

1. Get the trade catalog identifier using [catalog.catalog.list](../catalog/catalog-catalog-list.md)
2. Create a section using [catalog.section.add](./catalog-section-add.md)
3. Check the section using [catalog.section.get](./catalog-section-get.md) or [catalog.section.list](./catalog-section-list.md)
4. Bind products to the section using the required product method group

## Sections in REST 3.0

In [REST 3.0](../../rest-v3.md), catalog sections have methods for adding, updating, retrieving, and deleting, as well as the `catalog.section.field.list` and `catalog.section.field.get` methods for retrieving the field schema. Methods available in both API versions are marked `v 3.0` in the table.

Specifics of the new version methods:

- the [catalog.section.list](./catalog-section-list-rest-v3.md) method requires a condition on `iblockId` with the `=` operator in `filter`
- the [catalog.section.delete](./catalog-section-delete-rest-v3.md) method deletes a section together with all nested sections
- a top-level section has `iblockSectionId` equal to `0`; empty string fields are returned as an empty string, not `null`
- for a non-existent section or information block, the methods return the `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION` access error, not a "not found" error

## Relationship with Other Objects

**Trade Catalog.** Specify which trade catalog the section belongs to. You can obtain a list of catalogs using the method [catalog.catalog.list](../catalog/catalog-catalog-list.md).

**Products.** Link products and services to the necessary sections using the following groups of methods:

- [catalog.product.*](../product/index.md) — for simple products
- [catalog.product.service.*](../product/service/index.md) — for services
- [catalog.product.sku.*](../product/sku/index.md) — for parent products with variations
- [catalog.product.offer.*](../product/offer/index.md) — for product variations

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the methods:
> - administrator — for methods of the previous API version
> - a user with the "View product catalog" or "View catalog products" access permission and permissions for the information block sections — for REST 3.0 methods. Changing sections also requires the "Create product", "Edit product", or "Delete product" permission

#|
|| **Method** | **Description** ||
|| [catalog.section.add](./catalog-section-add.md) | Adds a section to the trade catalog ||
|| [catalog.section.add](./catalog-section-add-rest-v3.md) | Adds a section to the trade catalog v 3.0 ||
|| [catalog.section.update](./catalog-section-update.md) | Modifies the fields of a trade catalog section ||
|| [catalog.section.update](./catalog-section-update-rest-v3.md) | Modifies the fields of a trade catalog section v 3.0 ||
|| [catalog.section.get](./catalog-section-get.md) | Returns the values of the fields of a trade catalog section ||
|| [catalog.section.get](./catalog-section-get-rest-v3.md) | Returns the values of the fields of a trade catalog section v 3.0 ||
|| [catalog.section.list](./catalog-section-list.md) | Returns a list of sections in the trade catalog ||
|| [catalog.section.list](./catalog-section-list-rest-v3.md) | Returns a list of sections in the trade catalog v 3.0 ||
|| [catalog.section.delete](./catalog-section-delete.md) | Deletes a section from the trade catalog ||
|| [catalog.section.delete](./catalog-section-delete-rest-v3.md) | Deletes a section from the trade catalog together with its nested sections v 3.0 ||
|| [catalog.section.getFields](./catalog-section-get-fields.md) | Returns the fields of a trade catalog section ||
|| [catalog.section.field.list](./catalog-section-field-list.md) | Returns a list of trade catalog section fields v 3.0 ||
|| [catalog.section.field.get](./catalog-section-field-get.md) | Returns the description of a trade catalog section field ||
|#
