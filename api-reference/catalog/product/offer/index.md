# Product Variations: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Variations are different versions of a single product that differ by characteristics, such as color or size. Each variation has its own price, image, and stock quantity.

A variation is linked to a parent product, which contains general information: name, description, and category.

To work with parent products, use the methods [catalog.product.sku.*](../sku/index.md).

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Working with the product variants](https://helpdesk.bitrix24.com/open/18839102/)

## Relationship with Other Objects

**Trade Catalog.** A product variation is always linked to a specific trade catalog. You can obtain the identifiers of available trade catalogs using the method [catalog.catalog.list](../../catalog/catalog-catalog-list.md).

**Sections of the Trade Catalog.** Product variations are distributed across sections. To create and manage sections, use the group of methods [catalog.section.*](../../section/index.md).

**Images.** Product variations can contain images: for the announcement, detailed, and additional. To add images, use the methods [catalog.productImage.*](../../product-image/index.md), and to download, use the method [catalog.product.offer.download](./catalog-product-offer-download.md).

**Units of Measurement.** For a product variation, you specify the unit of measurement: piece, kilogram, or meter. You can add or change the unit of measurement using the methods [catalog.measure.*](../../measure/index.md).

**VAT.** The VAT rate can be set for each variation separately. You can work with rates through the methods [catalog.vat.*](../../vat/index.md).

**User.** Each variation stores the identifiers of users who created or modified it. You can obtain user information using [user.get](../../../user/user-get.md) and [user.search](../../../user/user-search.md).

**Product and Variation Properties.** Variations have properties that distinguish them from each other. These can include color, size, or material. You can work with properties using the methods [catalog.productProperty.*](../../product-property/index.md).

**Price Types.** Price types allow you to manage different pricing categories. A single product can have multiple prices: wholesale, retail, partner. To manage price types, use the methods [catalog.priceType.*](../../price-type/index.md).

**Price.** Specify the product price using the methods [catalog.price.*](../../price/index.md).

**Currencies.** For the product price, you select a currency. You can work with currencies through the methods [crm.currency.*](../../../crm/currency/index.md).

**Inventory Management.** If inventory management is enabled, you do not need to specify the available quantity of the product variation. Values will be set automatically from inventory documents. To work with documents, use the methods [catalog.document.*](../../document/index.md).

**CRM.** A variation can be added to the list of products in a [lead](../../../crm/leads/index.md), [deal](../../../crm/deals/index.md), [invoice](../../../crm/universal/invoice.md), [SPA](../../../crm/universal/index.md), and [estimate](../../../crm/quote/index.md).

**Order Cart.** A product variation can be added, modified, or removed from the cart using the [sale.basketitem.*](../../../sale/basket-item/index.md) method group.

## How to Start

1. Get the trade catalog identifier for variations using [catalog.catalog.list](../../catalog/catalog-catalog-list.md)
2. Get the parent product identifier using [catalog.product.sku.list](../sku/catalog-product-sku-list.md)
3. Create a variation using [catalog.product.offer.add](./catalog-product-offer-add.md)
4. Add images using [catalog.productImage.add](../../product-image/catalog-product-image-add.md), if needed
5. Set variation prices using [catalog.price.*](../../price/index.md)

## Variations in REST 3.0

In [REST 3.0](../../../rest-v3.md), variations have methods for adding, updating, retrieving, deleting, and downloading files, as well as the `catalog.product.offer.field.list` and `catalog.product.offer.field.get` methods for retrieving the field schema. Methods available in both API versions are marked `v 3.0` in the table. The general specifics of the new version product methods are described in the [products overview](../index.md).

Specifics of the new version methods:

- a variation is linked to its parent product through the `PROPERTY_{skuPropertyId}` property in `fields.properties`. There is no `parentId` field, unlike in the previous version. The `skuPropertyId` property identifier is returned by the [catalog.catalog.get](../../catalog/catalog-catalog-get-rest-v3.md) method for the variations catalog
- in the [catalog.product.offer.update](./catalog-product-offer-update-rest-v3.md) method, pass the link as an object `{"VALUE": id}`: the method does not save a number
- variations of a single parent product cannot be found by a filter on the link property

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the methods:
> - administrator — for methods of the previous API version
> - a user with the "View product catalog", "View catalog products", or "Edit selling price" access permission and permissions for the information block — for retrieving variations in REST 3.0. Adding, updating, and deleting also require the "Create product", "Edit product", or "Delete product" access permission

#|
|| **Method** | **Description** ||
|| [catalog.product.offer.add](./catalog-product-offer-add.md) | Adds a product variation ||
|| [catalog.product.offer.add](./catalog-product-offer-add-rest-v3.md) | Adds a product variation v 3.0 ||
|| [catalog.product.offer.update](./catalog-product-offer-update.md) | Updates fields of a product variation ||
|| [catalog.product.offer.update](./catalog-product-offer-update-rest-v3.md) | Updates fields of a product variation v 3.0 ||
|| [catalog.product.offer.get](./catalog-product-offer-get.md) | Returns field values of a product variation by identifier ||
|| [catalog.product.offer.get](./catalog-product-offer-get-rest-v3.md) | Returns field values of a product variation by identifier v 3.0 ||
|| [catalog.product.offer.list](./catalog-product-offer-list.md) | Returns a list of product variations by filter ||
|| [catalog.product.offer.list](./catalog-product-offer-list-rest-v3.md) | Returns a list of product variations by filter v 3.0 ||
|| [catalog.product.offer.download](./catalog-product-offer-download.md) | Downloads files of a product variation based on provided parameters ||
|| [catalog.product.offer.download](./catalog-product-offer-download-rest-v3.md) | Downloads files of a product variation based on provided parameters v 3.0 ||
|| [catalog.product.offer.delete](./catalog-product-offer-delete.md) | Deletes a product variation ||
|| [catalog.product.offer.delete](./catalog-product-offer-delete-rest-v3.md) | Deletes a product variation v 3.0 ||
|| [catalog.product.offer.getFieldsByFilter](./catalog-product-offer-get-fields-by-filter.md) | Returns fields of a product variation by filter ||
|| [catalog.product.offer.field.list](./catalog-product-offer-field-list.md) | Returns a list of product variation fields v 3.0 ||
|| [catalog.product.offer.field.get](./catalog-product-offer-field-get.md) | Returns the description of a product variation field ||
|#
