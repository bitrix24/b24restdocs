# Sections in the Trade Catalog: Overview of Methods

Sections are used to organize products and services in the catalog.

A product or service can be linked to multiple sections. For example, the product "Bandana" can appear in the sections "For Sports," "Accessories," and "Headwear." One of the sections must be marked as primary to prevent search engines from considering the product "Bandana" as duplicate content.

> Quick navigation: [all methods](#all-methods)
> 
> User documentation: [How to create and configure a product catalog](https://helpdesk.bitrix24.com/open/21121728/)

## Linking Sections with Other Objects

**Trade Catalog.** Specify which trade catalog the section belongs to. You can obtain a list of catalogs using the method [catalog.catalog.list](../catalog/catalog-catalog-list.md).

**Products.** Link products and services to the necessary sections using the following groups of methods:
- [catalog.product.*](../product/index.md) — for simple products
- [catalog.product.service.*](../product/service/index.md) — for services
- [catalog.product.sku.*](../product/sku/index.md) — for parent products with variations
- [catalog.product.offer.*](../product/offer/index.md) — for product variations

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the methods: administrator

#|
|| **Method** | **Description** ||
|| [catalog.section.add](./catalog-section-add.md) | Adds a section to the trade catalog ||
|| [catalog.section.update](./catalog-section-update.md) | Modifies the fields of a trade catalog section ||
|| [catalog.section.get](./catalog-section-get.md) | Returns the values of the fields of a trade catalog section ||
|| [catalog.section.list](./catalog-section-list.md) | Returns a list of sections in the trade catalog ||
|| [catalog.section.delete](./catalog-section-delete.md) | Deletes a section from the trade catalog ||
|| [catalog.section.getFields](./catalog-section-get-fields.md) | Returns the fields of a trade catalog section ||
|#