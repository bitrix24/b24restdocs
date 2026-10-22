# Parent Products: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A parent product is a product with variations. For example, a t-shirt is a parent product, while a blue t-shirt in size M and a white t-shirt in size L are its variations. A parent product is not a standalone item. It consolidates variations and stores common information: name, description, category. Price, currency, VAT, unit of measurement, and stock quantity are specified separately for each variation.

Differences between a parent product and a simple product:

- A parent product cannot be purchased directly; one must select one of its variations. To work with variations, use the group of methods [catalog.product.offer.*](../offer/index.md).

- A simple product is a single item without variations. It can be added to the cart and purchased immediately. To work with simple products, use the group of methods [catalog.product.*](../index.md).

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Add products to the catalog](https://helpdesk.bitrix24.com/open/18532198/)

## Relationship with Other Objects

**Trade Catalog.** A product must be linked to a specific trade catalog. You can obtain the identifiers of available trade catalogs using the method [catalog.catalog.list](../../catalog/catalog-catalog-list.md).

**Sections of the Trade Catalog.** Parent products are usually distributed across sections. To create and manage sections, use the [catalog.section.*](../../section/index.md) method group.

**Images.** A parent product can contain images: for the announcement, detailed view, and additional images. To add images, use the methods [catalog.productImage.*](../../product-image/index.md), and to download, use the method [catalog.product.sku.download](./catalog-product-sku-download.md).

**User.** Each product stores the identifiers of users who created and modified it. User information can be obtained using [user.get](../../../user/user-get.md) and [user.search](../../../user/user-search.md).

**Product and Variation Properties.** Parent products can have additional properties: manufacturer, season, or material. You can work with properties using the methods [catalog.productProperty.*](../../product-property/index.md).

**CRM.** In parent products, you can specify [leads](../../../crm/leads/index.md), [deals](../../../crm/deals/index.md), [SPAs](../../../crm/universal/index.md), [invoices](../../../crm/universal/invoice.md), [contacts](../../../crm/contacts/index.md), and [companies](../../../crm/companies/index.md) using the property type "Link to CRM entities."

## How to Start

1. Get the trade catalog identifier using [catalog.catalog.list](../../catalog/catalog-catalog-list.md)
2. Create a parent product using [catalog.product.sku.add](./catalog-product-sku-add.md)
3. Create variations for the parent product using [catalog.product.offer.add](../offer/catalog-product-offer-add.md)
4. Add images using [catalog.productImage.add](../../product-image/catalog-product-image-add.md), if needed
5. Set variation prices using [catalog.price.*](../../price/index.md)

## Parent Products in REST 3.0

In [REST 3.0](../../../rest-v3.md), parent products have methods for adding, updating, retrieving, deleting, and downloading files, as well as the `catalog.product.sku.field.list` and `catalog.product.sku.field.get` methods for retrieving the field schema. Methods available in both API versions are marked `v 3.0` in the table. The general specifics of the new version product methods are described in the [products overview](../index.md).

Specifics of the new version methods:

- a parent product without variations gets type `6`, and after the first variation is added, type `3`. If the last variation is deleted, the parent product becomes a simple product and is no longer returned by the `catalog.product.sku.*` methods
- the [catalog.product.sku.delete](./catalog-product-sku-delete-rest-v3.md) method deletes the parent product together with all its variations

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the methods:
> - administrator — for methods of the previous API version
> - a user with the "View product catalog", "View catalog products", or "Edit selling price" access permission and permissions for the information block — for retrieving parent products in REST 3.0. Adding, updating, and deleting also require the "Create product", "Edit product", or "Delete product" access permission

#|
|| **Method** | **Description** ||
|| [catalog.product.sku.add](./catalog-product-sku-add.md) | Adds a parent product to the trade catalog ||
|| [catalog.product.sku.add](./catalog-product-sku-add-rest-v3.md) | Adds a parent product to the trade catalog v 3.0 ||
|| [catalog.product.sku.update](./catalog-product-sku-update.md) | Updates fields of the parent product ||
|| [catalog.product.sku.update](./catalog-product-sku-update-rest-v3.md) | Updates fields of the parent product v 3.0 ||
|| [catalog.product.sku.get](./catalog-product-sku-get.md) | Returns field values of the parent product by identifier ||
|| [catalog.product.sku.get](./catalog-product-sku-get-rest-v3.md) | Returns field values of the parent product by identifier v 3.0 ||
|| [catalog.product.sku.list](./catalog-product-sku-list.md) | Returns a list of parent products by filter ||
|| [catalog.product.sku.list](./catalog-product-sku-list-rest-v3.md) | Returns a list of parent products by filter v 3.0 ||
|| [catalog.product.sku.download](./catalog-product-sku-download.md) | Downloads files of the parent product based on the provided parameters ||
|| [catalog.product.sku.download](./catalog-product-sku-download-rest-v3.md) | Downloads files of the parent product based on the provided parameters v 3.0 ||
|| [catalog.product.sku.delete](./catalog-product-sku-delete.md) | Deletes a parent product ||
|| [catalog.product.sku.delete](./catalog-product-sku-delete-rest-v3.md) | Deletes a parent product v 3.0 ||
|| [catalog.product.sku.getFieldsByFilter](./catalog-product-sku-get-fields-by-filter.md) | Returns fields of the parent product by filter ||
|| [catalog.product.sku.field.list](./catalog-product-sku-field-list.md) | Returns a list of parent product fields v 3.0 ||
|| [catalog.product.sku.field.get](./catalog-product-sku-field-get.md) | Returns the description of a parent product field ||
|#
