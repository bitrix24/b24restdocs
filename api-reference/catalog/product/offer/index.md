# Product Variations: Overview of Methods

Variations are different versions of a single product that differ in color and size. Each variation has its own price, image, and stock quantity.

A variation is linked to a parent product, which contains general information: name, description, and category.

To work with parent products, use the methods [catalog.product.sku.*](../sku/index.md).

> Quick navigation: [all methods](#all-methods) 
> 
> User documentation: [Working with the product variants](https://helpdesk.bitrix24.com/open/18839102/)

## Relationship of Product Variations with Other Objects

**Trade Catalog.** A product variation is always linked to a specific trade catalog. You can obtain the identifiers of available trade catalogs using the method [catalog.catalog.list](../../catalog/catalog-catalog-list.md).

**Sections of the Trade Catalog.** Product variations are distributed across sections. To create and manage sections, use the group of methods [catalog.section.*](../../section/index.md).

**Images.** Product variations can contain images: for the announcement, detailed, and additional. To add images, use the methods [catalog.productImage.*](../../product-image/index.md), and to download, use the method [catalog.product.offer.download](./catalog-product-offer-download.md).

**Units of Measurement.** For a product variation, you specify the unit of measurement: piece, kilogram, or meter. You can add or change the unit of measurement using the methods [catalog.measure.*](../../measure/index.md).

**VAT.** The VAT rate can be set for each variation separately. You can work with rates through the methods [catalog.vat.*](../../vat/index.md).

**User.** Each variation stores the identifiers of users who created or modified it. You can obtain user information using the methods [user.get](../../../user/user-get.md) and [user.search](../../../user/user-search.md).

**Product and Variation Properties.** Variations have properties that distinguish them from each other. These can include color, size, or material. You can work with properties using the methods [catalog.productProperty.*](../../product-property/index.md).

**Price Types.** Price types allow you to manage different pricing categories. A single product can have multiple prices: wholesale, retail, partner. To manage price types, use the methods [catalog.priceType.*](../../price-type/index.md).

**Price.** Specify the product price using the methods [catalog.price.*](../../price/index.md).

**Currencies.** For the product price, you select a currency. You can work with currencies through the methods [crm.currency.*](../../../crm/currency/index.md).

**Inventory Management.** If inventory management is enabled, you do not need to specify the available quantity of the product variation. Values will be set automatically from inventory documents. To work with documents, use the methods [catalog.document.*](../../document/index.md).

**CRM.** A variation can be added to the list of products in a [lead](../../../crm/leads/index.md), [deal](../../../crm/deals/index.md), [invoice](../../../crm/universal/invoice.md), [SPA](../../../crm/universal/index.md), and [estimate](../../../crm/quote/index.md).

**Order Cart.** A product variation can be added, modified, or removed from the cart using the group of methods [sale.basketitem.*](../../../sale/basket-item/index.md).

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

#|
|| **Method** | **Description** ||
|| [catalog.product.offer.add](./catalog-product-offer-add.md) | Adds a product variation ||
|| [catalog.product.offer.update](./catalog-product-offer-update.md) | Updates fields of a product variation ||
|| [catalog.product.offer.get](./catalog-product-offer-get.md) | Returns field values of a product variation by identifier ||
|| [catalog.product.offer.list](./catalog-product-offer-list.md) | Returns a list of product variations by filter ||
|| [catalog.product.offer.download](./catalog-product-offer-download.md) | Downloads files of a product variation based on provided parameters ||
|| [catalog.product.offer.delete](./catalog-product-offer-delete.md) | Deletes a product variation ||
|| [catalog.product.offer.getFieldsByFilter](./catalog-product-offer-get-fields-by-filter.md) | Returns fields of a product variation by filter ||
|#