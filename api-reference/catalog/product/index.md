# Products in the Trade Catalog: Overview of Methods

Using REST methods, you can create simple products or services, as well as products with variations.

A simple product or service is a single inventory item with a name and unit of measurement. A product variation is a commercial offer where the product has additional characteristics: color and size. A separate inventory item with a unique SKU is created for each combination of characteristics.

> Quick navigation: [all methods](#all-methods)
> 
> User documentation: 
>   - [How to create a new product in the catalog](https://helpdesk.bitrix24.com/open/18532198/)
>   - [Working with product variations](https://helpdesk.bitrix24.com/open/18839102/)

## Linking Products to Other Objects

**Trade Catalog.** A product must be linked to a specific trade catalog. You can obtain the identifiers of available trade catalogs using the [catalog.catalog.list](../catalog/catalog-catalog-list.md) method.

**Sections of the Trade Catalog.** Products are typically distributed across sections. To create and manage sections, use the group of methods [catalog.section.* ](../section/index.md).

**Units of Measurement.** You can add the necessary units of measurement using the [catalog.measure.* ](../measure/index.md) methods.

**Product and Variation Images.** To help customers understand what the product is, add product images: a detailed picture, a preview image, and additional images. Use the [catalog.productImage.*](../product-image/index.md) methods for this.

**Product and Variation Properties.** All products and variations in the online store have a set of properties that distinguish them from each other, such as color or size. These properties help customers search for and select products on the site. Create properties using the [catalog.productProperty.*](../product-property/index.md) methods.

**Price Types.** Price types allow you to manage different pricing categories. A single product can have multiple prices: wholesale, retail, partner. Use the [catalog.priceType.*](../price-type/index.md) methods to manage price types.

**Price.** Specify the product price using the [catalog.price.*](../price/index.md) methods.

**Inventory Management.** If inventory management is enabled, you do not need to specify the available quantity when creating or editing a product — inventory management will automatically set the values.

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can perform the methods: administrator

### Products

#|
|| **Method** | **Description** ||
|| [catalog.product.add](./catalog-product-add.md) | Adds a product ||
|| [catalog.product.update](./catalog-product-update.md) | Updates product fields ||
|| [catalog.product.get](./catalog-product-get.md) | Returns product field values by identifier ||
|| [catalog.product.list](./catalog-product-list.md) | Returns a list of products by filter ||
|| [catalog.product.download](./catalog-product-download.md) | Downloads product files by provided parameters ||
|| [catalog.product.delete](./catalog-product-delete.md) | Deletes a product ||
|| [catalog.product.getFieldsByFilter](./catalog-product-get-fields-by-filter.md) | Returns product fields by filter ||
|#

### Services

#|
|| **Method** | **Description** ||
|| [catalog.product.service.add](./service/catalog-product-service-add.md) | Adds a service ||
|| [catalog.product.service.update](./service/catalog-product-service-update.md) | Updates service fields ||
|| [catalog.product.service.get](./service/catalog-product-service-get.md) | Returns service field values by identifier ||
|| [catalog.product.service.list](./service/catalog-product-service-list.md) | Returns a list of services by filter ||
|| [catalog.product.service.download](./service/catalog-product-service-download.md) | Downloads service files by provided parameters ||
|| [catalog.product.service.delete](./service/catalog-product-service-delete.md) | Deletes a service ||
|| [catalog.product.service.getFieldsByFilter](./service/catalog-product-service-get-fields-by-filter.md) | Returns service fields by filter ||
|#

### Products with Variations: Parent Products

#|
|| **Method** | **Description** ||
|| [catalog.product.sku.add](./sku/catalog-product-sku-add.md) | Adds a parent product ||
|| [catalog.product.sku.update](./sku/catalog-product-sku-update.md) | Updates parent product fields ||
|| [catalog.product.sku.get](./sku/catalog-product-sku-get.md) | Returns parent product field values by identifier ||
|| [catalog.product.sku.list](./sku/catalog-product-sku-list.md) | Returns a list of parent products by filter ||
|| [catalog.product.sku.download](./sku/catalog-product-sku-download.md) | Downloads parent product files by provided parameters ||
|| [catalog.product.sku.delete](./sku/catalog-product-sku-delete.md) | Deletes a parent product ||
|| [catalog.product.sku.getFieldsByFilter](./sku/catalog-product-sku-get-fields-by-filter.md) | Returns parent product fields by filter ||
|#

### Products with Variations: Variations

#|
|| **Method** | **Description** ||
|| [catalog.product.offer.add](./offer/catalog-product-offer-add.md) | Adds a product variation ||
|| [catalog.product.offer.update](./offer/catalog-product-offer-update.md) | Updates product variation fields ||
|| [catalog.product.offer.get](./offer/catalog-product-offer-get.md) | Returns product variation field values by identifier ||
|| [catalog.product.offer.list](./offer/catalog-product-offer-list.md) | Returns a list of product variations by filter ||
|| [catalog.product.offer.download](./offer/catalog-product-offer-download.md) | Downloads product variation files by provided parameters ||
|| [catalog.product.offer.delete](./offer/catalog-product-offer-delete.md) | Deletes a product variation ||
|| [catalog.product.offer.getFieldsByFilter](./offer/catalog-product-offer-get-fields-by-filter.md) | Returns product variation fields by filter ||
|#