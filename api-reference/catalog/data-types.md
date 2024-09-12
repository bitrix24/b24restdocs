# Data Types and Object Structure in the REST API Catalog

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- Create descriptions for objects catalog_vat, rest_field_description

{% endnote %}

{% endif %}

Basic data types are listed in a separate [article](../data-types.md).

In this article, we will discuss the data types and object structure specific to the CRM Catalog.

## Data Types

#|
|| **Type** | **Descriptions and Values** ||
|| [`catalog_catalog.id`](#catalog_catalog) | Integer identifier of the trade catalog (e.g., `1`). You can obtain the identifiers of trade catalogs using the method [catalog.catalog.list](./catalog/catalog-catalog-list.md) ||
|| [`catalog_product.id`](#catalog_product) | Integer identifier of the product (e.g., `1`). You can obtain the identifiers of products using the method [catalog.product.list](./product/catalog-product-list.md) ||
|| [`catalog_product_sku.id`](#catalog_product_sku) | Integer identifier of the parent product (e.g., `1`). You can obtain the identifiers of parent products using the method [catalog.product.sku.list](./product/sku/catalog-product-sku-list.md) ||
|| [`catalog_product_offer.id`](#catalog_product_offer) | Integer identifier of the trade offer (e.g., `1`). You can obtain the identifiers of trade offers using the method [catalog.product.offer.list](./product/offer/catalog-product-offer-list.md) ||
|| [`catalog_product_service.id`](#catalog_product_service) | Integer identifier of the service (e.g., `1`). You can obtain the identifiers of services using the method [catalog.product.service.list](./product/service/catalog-product-service-list.md) ||
|| [`catalog_measure`](#catalog_measure) | Integer identifier of the unit of measurement (e.g., `1`). You can obtain the identifiers of units of measurement using the method [catalog.measure.list](./measure/catalog-measure-list.md) ||
|| [`catalog_section`](#catalog_section) | Integer identifier of the catalog section (e.g., `1`). You can obtain the identifiers of product sections using the method [catalog.section.list](./section/catalog-section-list.md) ||
|#

## Object Structure

### catalog_catalog

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the trade catalog ||
|| **iblockId**
[`integer`](../data-types.md) | Identifier of the information block of the trade catalog ||
|| **iblockTypeId**
[`string`](../data-types.md) | Type of the information block of the trade catalog. For CRM trade catalogs, it has a constant value of `CRM_PRODUCT_CATALOG` ||
|| **lid**
[`string`](../data-types.md) | Site identifier. Has a constant value of `s1` ||
|| **name**
[`string`](../data-types.md) | Name of the trade catalog ||
|| **productIblockId**
[`integer`](../data-types.md) | Identifier of the parent information block of the trade catalog. Filled only for the trade catalog of trade offers.

To obtain existing identifiers of information blocks, you need to use [catalog.catalog.list](./catalog/catalog-catalog-list.md)
||
|| **skuPropertyId**
[`integer`](../data-types.md) | Identifier of the property that stores the identifier of the parent product. Filled only for the trade catalog of trade offers.

To obtain existing property identifiers, you need to use [catalog.productProperty.list](./product-property/catalog-product-property-list.md)
||
|| **subscription**
[`string`](../data-types.md) | Is content being sold? Possible values:
- `Y` — yes
- `N` — no

This parameter is used only in the on-premise version
||
|| **vatId**
[`catalog_vat.id`](#catalog_vat) | VAT identifier.

To obtain existing VAT identifiers, you need to use [catalog.vat.list](./vat/catalog-vat-list.md)
||
|| **yandexExport**
[`string`](../data-types.md) | Deprecated parameter. Should it be exported to *Yandex.Market*? Possible values:
- `Y` — yes
- `N` — no ||
|#

### catalog_product

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the product ||
|| **iblockId**
[`catalog_catalog.id`](#catalog_catalog) | Identifier of the information block of the trade catalog.

To obtain existing identifiers of information blocks, you need to use [catalog.catalog.list](./catalog/catalog-catalog-list.md)
||
|| **name**
[`string`](../data-types.md) | Name of the product ||
|| **active**
[`string`](../data-types.md) | Activity status. Possible values:
- `Y` — yes
- `N` — no
||
|| **available**
[`string`](../data-types.md) | Availability for purchase. Read-only. Possible values:
- `Y` — yes
- `N` — no
||
|| **code**
[`string`](../data-types.md) | Symbolic code ||
|| **xmlId**
[`string`](../data-types.md) | External code ||
|| **barcodeMulti**
[`string`](../data-types.md) | Unique barcodes for each instance. Possible values:
- `Y` — yes
- `N` — no 
||
|| **bundle**
[`string`](../data-types.md) | Availability of a set. Possible values:
- `Y` — yes
- `N` — no 
||
|| **canBuyZero**
[`string`](../data-types.md) | Is the purchase of the product allowed when it is out of stock? Possible values:
- `Y` — yes
- `N` — no 
||
|| **createdBy**
[`user.id`](../data-types.md) | Created by ||
|| **modifiedBy**
[`user.id`](../data-types.md) | Modified by ||
|| **dateActiveFrom**
[`datetime`](../data-types.md) | Date of activation start ||
|| **dateActiveTo**
[`datetime`](../data-types.md) | Date of activation end ||
|| **dateCreate**
[`datetime`](../data-types.md) | Date of creation ||
|| **timestampX**
[`datetime`](../data-types.md) | Date of modification. Read-only ||
|| **iblockSectionId**
[`catalog_section.id`](#catalog_section) | Identifier of the information block section ||
|| **measure**
[`catalog_measure.id`](#catalog_measure) | Unit of measurement ||
|| **previewText**
[`string`](../data-types.md) | Description for the announcement ||
|| **detailText**
[`string`](../data-types.md) | Detailed description ||
|| **previewPicture**
[`object`](../data-types.md) | Picture for the announcement. Object in the format `{fileData: [value1, value2]}`, where `value1` — name of the picture file with extension, `value2` — picture in base64 format. 

To delete the picture, use the object in the format `{remove: ‘Y’}` ||
|| **detailPicture**
[`object`](../data-types.md) | Detailed picture. Object in the format `{fileData: [value1, value2]}`, where `value1` — name of the picture file with extension, `value2` — picture in base64 format. 

To delete the picture, use the object in the format `{remove: ‘Y’}` ||
|| **previewTextType**
[`string`](../data-types.md) | Type of description for the announcement. Possible values:
- `text` — text
- `html` — HTML
||
|| **detailTextType**
[`string`](../data-types.md) | Type of detailed description. Possible values:
- `text` — text
- `html` — HTML
||
|| **sort**
[`integer`](../data-types.md) | Sorting ||
|| **subscribe**
[`string`](../data-types.md) | Permission to subscribe to the product. Possible values:
- `Y` — yes
- `N` — no
- `D` — default
||
|| **vatId**
[`catalog_vat.id`](#catalog_vat) | VAT identifier ||
|| **vatIncluded**
[`string`](../data-types.md) | Is VAT included in the price? Possible values:
- `Y` — yes
- `N` — no
||
|| **height**
[`float`](../data-types.md) | Height of the product ||
|| **length**
[`float`](../data-types.md) | Length of the product ||
|| **weight**
[`float`](../data-types.md) | Weight of the product ||
|| **width**
[`float`](../data-types.md) | Width of the product ||
|| **quantityTrace**
[`string`](../data-types.md) | Quantity accounting mode. Possible values:
- `Y` — enabled
- `N` — disabled
- `D` — default
||
|| **type**
[`integer`](../data-types.md) | Type of product. Read-only. Possible values:
- `1` — simple product
||
|| **purchasingCurrency**
[`string`](../data-types.md) | Currency of the purchasing price.

The list of currencies can be obtained using the method [crm.currency.list](../crm/currency/crm-currency-list.md).

Not editable when inventory accounting is enabled
||
|| **purchasingPrice**
[`float`](../data-types.md) | Purchasing price.

Not editable when inventory accounting is enabled
||
|| **quantity**
[`float`](../data-types.md) | Quantity.

Not editable when inventory accounting is enabled
||
|| **quantityReserved**
[`float`](../data-types.md) | Reserved quantity.

Not editable when inventory accounting is enabled
||
|| **recurSchemeLength**
[`integer`](../data-types.md) | Length of the payment period.

Used only in the on-premise version for content sales ||
|| **recurSchemeType**
[`string`](../data-types.md) | Unit of time for the payment period. Possible values:
- `H` — hour
- `D` — day
- `W` — week
- `M` — month
- `Q` — quarter
- `S` — half-year
- `Y` — year

Used only in the on-premise version for content sales
||
|| **trialPriceId**
[`integer`](../data-types.md) | Product for trial payment.

Used only in the on-premise version for content sales ||
|| **withoutOrder**
[`string`](../data-types.md) | Renewal without placing an order. Possible values:
- `Y` — yes
- `N` — no

Used only in the on-premise version for content sales
||
|| **propertyN**
[`object\|array`](../data-types.md) | Value of the product property, where `N` — identifier of the property. There can be multiple properties. 

The value is specified in the format `{valueId: valueId, value: value}` or in the format `[{valueId: valueId1, value: value1}, ..., {valueId: valueIdN, value: valueN}]` if the property is multiple. Here `valueId` — identifier of the property value, and `value` — property value. 

If `valueId` is not specified, the existing value will be removed from the database and replaced with the new one specified in `value`. If the property is multiple, all existing property values for which `valueId` was not specified will be removed.

`valueId` of all product properties can be obtained using the methods [catalog.product.get](./product/catalog-product-get.md) and [catalog.product.list](./product/catalog-product-list.md)
||
|#

### catalog_product_sku

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the parent product ||
|| **iblockId**
[`catalog_catalog.id`](#catalog_catalog) | Identifier of the information block of the trade catalog.

To obtain existing identifiers of information blocks, you need to use [catalog.catalog.list](./catalog/catalog-catalog-list.md)
||
|| **name**
[`string`](../data-types.md) | Name of the parent product ||
|| **active**
[`string`](../data-types.md) | Activity status. Possible values:
- `Y` — yes
- `N` — no
||
|| **available**
[`string`](../data-types.md) | Availability for purchase. Read-only. Possible values:
- `Y` — yes
- `N` — no
||
|| **code**
[`string`](../data-types.md) | Symbolic code ||
|| **xmlId**
[`string`](../data-types.md) | External code ||
|| **bundle**
[`string`](../data-types.md) | Availability of a set. Possible values:
- `Y` — yes
- `N` — no 
||
|| **canBuyZero**
[`string`](../data-types.md) | Is the purchase of the parent product allowed when it is out of stock? Possible values:
- `Y` — yes
- `N` — no 

For parent products, the ability to edit this field is available only in the on-premise version when the option "Show the Trade Catalog tab for products with trade offers" is enabled
||
|| **createdBy**
[`user.id`](../data-types.md) | Created by ||
|| **modifiedBy**
[`user.id`](../data-types.md) | Modified by ||
|| **dateActiveFrom**
[`datetime`](../data-types.md) | Date of activation start ||
|| **dateActiveTo**
[`datetime`](../data-types.md) | Date of activation end ||
|| **dateCreate**
[`datetime`](../data-types.md) | Date of creation ||
|| **timestampX**
[`datetime`](../data-types.md) | Date of modification. Read-only ||
|| **iblockSectionId**
[`catalog_section.id`](#catalog_section) | Identifier of the information block section ||
|| **measure**
[`catalog_measure.id`](#catalog_measure) | Unit of measurement.

For parent products, the ability to edit this field is available only in the on-premise version when the option "Show the Trade Catalog tab for products with trade offers" is enabled ||
|| **previewText**
[`string`](../data-types.md) | Description for the announcement ||
|| **detailText**
[`string`](../data-types.md) | Detailed description ||
|| **previewPicture**
[`object`](../data-types.md) | Picture for the announcement. Object in the format `{fileData: [value1, value2]}`, where `value1` — name of the picture file with extension, `value2` — picture in base64 format. 

To delete the picture, use the object in the format `{remove: ‘Y’}` ||
|| **detailPicture**
[`object`](../data-types.md) | Detailed picture. Object in the format `{fileData: [value1, value2]}`, where `value1` — name of the picture file with extension, `value2` — picture in base64 format. 

To delete the picture, use the object in the format `{remove: ‘Y’}` ||
|| **previewTextType**
[`string`](../data-types.md) | Type of description for the announcement. Possible values:
- `text` — text
- `html` — HTML
||
|| **detailTextType**
[`string`](../data-types.md) | Type of detailed description. Possible values:
- `text` — text
- `html` — HTML
||
|| **sort**
[`integer`](../data-types.md) | Sorting ||
|| **subscribe**
[`string`](../data-types.md) | Permission to subscribe to the parent product. Possible values:
- `Y` — yes
- `N` — no
- `D` — default

For parent products, the ability to edit this field is available only in the on-premise version when the option "Show the Trade Catalog tab for products with trade offers" is enabled
||
|| **vatId**
[`catalog_vat.id`](#catalog_vat) | VAT identifier.

For parent products, the ability to edit this field is available only in the on-premise version when the option "Show the Trade Catalog tab for products with trade offers" is enabled ||
|| **vatIncluded**
[`string`](../data-types.md) | Is VAT included in the price? Possible values:
- `Y` — yes
- `N` — no

For parent products, the ability to edit this field is available only in the on-premise version when the option "Show the Trade Catalog tab for products with trade offers" is enabled ||
|| **height**
[`float`](../data-types.md) | Height of the parent product.

For parent products, the ability to edit this field is available only in the on-premise version when the option "Show the Trade Catalog tab for products with trade offers" is enabled ||
|| **length**
[`float`](../data-types.md) | Length of the parent product.

For parent products, the ability to edit this field is available only in the on-premise version when the option "Show the Trade Catalog tab for products with trade offers" is enabled ||
|| **weight**
[`float`](../data-types.md) | Weight of the parent product.

For parent products, the ability to edit this field is available only in the on-premise version when the option "Show the Trade Catalog tab for products with trade offers" is enabled ||
|| **width**
[`float`](../data-types.md) | Width of the parent product.

For parent products, the ability to edit this field is available only in the on-premise version when the option "Show the Trade Catalog tab for products with trade offers" is enabled ||
|| **type**
[`integer`](../data-types.md) | Type of product. Read-only. Possible values:
- `3` — parent product with offers
- `6` — parent product without offers
||
|| **purchasingCurrency**
[`string`](../data-types.md) | Currency of the purchasing price.

The list of currencies can be obtained using the method [crm.currency.list](../crm/currency/crm-currency-list.md).

Not editable when inventory accounting is enabled.

For parent products, the ability to edit this field is available only in the on-premise version when the option "Show the Trade Catalog tab for products with trade offers" is enabled ||
|| **purchasingPrice**
[`float`](../data-types.md) | Purchasing price.

Not editable when inventory accounting is enabled.

For parent products, the ability to edit this field is available only in the on-premise version when the option "Show the Trade Catalog tab for products with trade offers" is enabled ||
|| **quantity**
[`float`](../data-types.md) | Quantity.

Not editable when inventory accounting is enabled.

For parent products, the ability to edit this field is available only in the on-premise version when the option "Show the Trade Catalog tab for products with trade offers" is enabled ||
|| **propertyN**
[`object\|array`](../data-types.md) | Value of the parent product property, where `N` — identifier of the property. There can be multiple properties. 

The value is specified in the format `{valueId: valueId, value: value}` or in the format `[{valueId: valueId1, value: value1}, ..., {valueId: valueIdN, value: valueN}]` if the property is multiple. Here `valueId` — identifier of the property value, and `value` — property value. 

If `valueId` is not specified, the existing value will be removed from the database and replaced with the new one specified in `value`. If the property is multiple, all existing property values for which `valueId` was not specified will be removed.

`valueId` of all parent product properties can be obtained using the methods [catalog.product.sku.get](./product/sku/catalog-product-sku-get.md) and [catalog.product.sku.list](./product/sku/catalog-product-sku-list.md)
||
|#

### catalog_product_offer

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the trade offer ||
|| **iblockId**
[`catalog_catalog.id`](#catalog_catalog) | Identifier of the information block of the trade catalog.

To obtain existing identifiers of information blocks, you need to use [catalog.catalog.list](./catalog/catalog-catalog-list.md)
||
|| **name**
[`string`](../data-types.md) | Name of the trade offer ||
|| **active**
[`string`](../data-types.md) | Activity status. Possible values:
- `Y` — yes
- `N` — no
||
|| **available**
[`string`](../data-types.md) | Availability for purchase. Read-only. Possible values:
- `Y` — yes
- `N` — no
||
|| **code**
[`string`](../data-types.md) | Symbolic code ||
|| **xmlId**
[`string`](../data-types.md) | External code ||
|| **barcodeMulti**
[`string`](../data-types.md) | Unique barcodes for each instance. Possible values:
- `Y` — yes
- `N` — no 
||
|| **bundle**
[`string`](../data-types.md) | Availability of a set. Possible values:
- `Y` — yes
- `N` — no 
||
|| **canBuyZero**
[`string`](../data-types.md) | Is the purchase of the product allowed when it is out of stock? Possible values:
- `Y` — yes
- `N` — no 
||
|| **createdBy**
[`user.id`](../data-types.md) | Created by ||
|| **modifiedBy**
[`user.id`](../data-types.md) | Modified by ||
|| **dateActiveFrom**
[`datetime`](../data-types.md) | Date of activation start ||
|| **dateActiveTo**
[`datetime`](../data-types.md) | Date of activation end ||
|| **dateCreate**
[`datetime`](../data-types.md) | Date of creation ||
|| **timestampX**
[`datetime`](../data-types.md) | Date of modification. Read-only ||
|| **iblockSectionId**
[`catalog_section.id`](#catalog_section) | Identifier of the information block section ||
|| **measure**
[`catalog_measure.id`](#catalog_measure) | Unit of measurement ||
|| **previewText**
[`string`](../data-types.md) | Description for the announcement ||
|| **detailText**
[`string`](../data-types.md) | Detailed description ||
|| **previewPicture**
[`object`](../data-types.md) | Picture for the announcement. Object in the format `{fileData: [value1, value2]}`, where `value1` — name of the picture file with extension, `value2` — picture in base64 format. 

To delete the picture, use the object in the format `{remove: ‘Y’}` ||
|| **detailPicture**
[`object`](../data-types.md) | Detailed picture. Object in the format `{fileData: [value1, value2]}`, where `value1` — name of the picture file with extension, `value2` — picture in base64 format. 

To delete the picture, use the object in the format `{remove: ‘Y’}` ||
|| **previewTextType**
[`string`](../data-types.md) | Type of description for the announcement. Possible values:
- `text` — text
- `html` — HTML
||
|| **detailTextType**
[`string`](../data-types.md) | Type of detailed description. Possible values:
- `text` — text
- `html` — HTML
||
|| **sort**
[`integer`](../data-types.md) | Sorting ||
|| **subscribe**
[`string`](../data-types.md) | Permission to subscribe to the trade offer. Possible values:
- `Y` — yes
- `N` — no
- `D` — default
||
|| **vatId**
[`catalog_vat.id`](#catalog_vat) | VAT identifier ||
|| **vatIncluded**
[`string`](../data-types.md) | Is VAT included in the price? Possible values:
- `Y` — yes
- `N` — no
||
|| **height**
[`float`](../data-types.md) | Height of the product ||
|| **length**
[`float`](../data-types.md) | Length of the product ||
|| **weight**
[`float`](../data-types.md) | Weight of the product ||
|| **width**
[`float`](../data-types.md) | Width of the product ||
|| **quantityTrace**
[`string`](../data-types.md) | Quantity accounting mode. Possible values:
- `Y` — enabled
- `N` — disabled
- `D` — default
||
|| **type**
[`integer`](../data-types.md) | Type of product. Read-only. Possible values:
- `4` — offer
- `5` — offer without product
||
|| **purchasingCurrency**
[`string`](../data-types.md) | Currency of the purchasing price.

The list of currencies can be obtained using the method [crm.currency.list](../crm/currency/crm-currency-list.md).

Not editable when inventory accounting is enabled
||
|| **purchasingPrice**
[`float`](../data-types.md) | Purchasing price.

Not editable when inventory accounting is enabled
||
|| **quantity**
[`float`](../data-types.md) | Quantity.

Not editable when inventory accounting is enabled
||
|| **quantityReserved**
[`float`](../data-types.md) | Reserved quantity.

Not editable when inventory accounting is enabled
||
|| **recurSchemeLength**
[`integer`](../data-types.md) | Length of the payment period.

Used only in the on-premise version for content sales ||
|| **recurSchemeType**
[`string`](../data-types.md) | Unit of time for the payment period. Possible values:
- `H` — hour
- `D` — day
- `W` — week
- `M` — month
- `Q` — quarter
- `S` — half-year
- `Y` — year

Used only in the on-premise version for content sales
||
|| **trialPriceId**
[`integer`](../data-types.md) | Product for trial payment.

Used only in the on-premise version for content sales ||
|| **withoutOrder**
[`string`](../data-types.md) | Renewal without placing an order. Possible values:
- `Y` — yes
- `N` — no

Used only in the on-premise version for content sales
||
|| **propertyN**
[`object\|array`](../data-types.md) | Value of the trade offer property, where `N` — identifier of the property. There can be multiple properties. 

The value is specified in the format `{valueId: valueId, value: value}` or in the format `[{valueId: valueId1, value: value1}, ..., {valueId: valueIdN, value: valueN}]` if the property is multiple. Here `valueId` — identifier of the property value, and `value` — property value. 

If `valueId` is not specified, the existing value will be removed from the database and replaced with the new one specified in `value`. If the property is multiple, all existing property values for which `valueId` was not specified will be removed.

`valueId` of all trade offer properties can be obtained using the methods [catalog.product.offer.get](./product/offer/catalog-product-offer-get.md) and [catalog.product.offer.list](./product/offer/catalog-product-offer-list.md)
||
|#

### catalog_product_service

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the service ||
|| **iblockId**
[`catalog_catalog.id`](#catalog_catalog) | Identifier of the information block of the trade catalog.

To obtain existing identifiers of information blocks, you need to use [catalog.catalog.list](./catalog/catalog-catalog-list.md)
||
|| **name**
[`string`](../data-types.md) | Name of the service ||
|| **active**
[`string`](../data-types.md) | Activity status. Possible values:
- `Y` — yes
- `N` — no
||
|| **available**
[`string`](../data-types.md) | Availability for purchase. Read-only. Possible values:
- `Y` — yes
- `N` — no
||
|| **code**
[`string`](../data-types.md) | Symbolic code ||
|| **xmlId**
[`string`](../data-types.md) | External code ||
|| **bundle**
[`string`](../data-types.md) | Availability of a set. Possible values:
- `Y` — yes
- `N` — no 
||
|| **createdBy**
[`user.id`](../data-types.md) | Created by ||
|| **modifiedBy**
[`user.id`](../data-types.md) | Modified by ||
|| **dateActiveFrom**
[`datetime`](../data-types.md) | Date of activation start ||
|| **dateActiveTo**
[`datetime`](../data-types.md) | Date of activation end ||
|| **dateCreate**
[`datetime`](../data-types.md) | Date of creation ||
|| **timestampX**
[`datetime`](../data-types.md) | Date of modification. Read-only ||
|| **iblockSectionId**
[`catalog_section.id`](#catalog_section) | Identifier of the information block section ||
|| **previewText**
[`string`](../data-types.md) | Description for the announcement ||
|| **detailText**
[`string`](../data-types.md) | Detailed description ||
|| **previewPicture**
[`object`](../data-types.md) | Picture for the announcement. Object in the format `{fileData: [value1, value2]}`, where `value1` — name of the picture file with extension, `value2` — picture in base64 format. 

To delete the picture, use the object in the format `{remove: ‘Y’}` ||
|| **detailPicture**
[`object`](../data-types.md) | Detailed picture. Object in the format `{fileData: [value1, value2]}`, where `value1` — name of the picture file with extension, `value2` — picture in base64 format. 

To delete the picture, use the object in the format `{remove: ‘Y’}` ||
|| **previewTextType**
[`string`](../data-types.md) | Type of description for the announcement. Possible values:
- `text` — text
- `html` — HTML
||
|| **detailTextType**
[`string`](../data-types.md) | Type of detailed description. Possible values:
- `text` — text
- `html` — HTML
||
|| **sort**
[`integer`](../data-types.md) | Sorting ||
|| **vatId**
[`catalog_vat.id`](#catalog_vat) | VAT identifier ||
|| **vatIncluded**
[`string`](../data-types.md) | Is VAT included in the price? Possible values:
- `Y` — yes
- `N` — no
||
|| **type**
[`integer`](../data-types.md) | Type of product. Read-only. Possible values:
- `7` — service
||
|| **propertyN**
[`object\|array`](../data-types.md) | Value of the service property, where `N` — identifier of the property. There can be multiple properties. 

The value is specified in the format `{valueId: valueId, value: value}` or in the format `[{valueId: valueId1, value: value1}, ..., {valueId: valueIdN, value: valueN}]` if the property is multiple. Here `valueId` — identifier of the property value, and `value` — property value. 

If `valueId` is not specified, the existing value will be removed from the database and replaced with the new one specified in `value`. If the property is multiple, all existing property values for which `valueId` was not specified will be removed.

`valueId` of all service properties can be obtained using the methods [catalog.product.service.get](./product/service/catalog-product-service-get.md) and [catalog.product.service.list](./product/service/catalog-product-service-list.md)
||
|#

### catalog_measure

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the unit of measurement ||
|| **code**
[`integer`](../data-types.md) | Code of the unit of measurement ||
|| **isDefault**
[`string`](../data-types.md) | Is the current unit of measurement used as the default unit for new products? Possible values:
- `Y` — yes
- `N` — no

Only one unit of measurement from the entire directory can have the value `Y`
||
|| **measureTitle**
[`string`](../data-types.md) | Name of the unit of measurement ||
|| **symbol**
[`string`](../data-types.md) | Conditional designation ||
|| **symbolIntl**
[`string`](../data-types.md) | International conditional designation ||
|| **symbolLetterIntl**
[`string`](../data-types.md) | International letter code designation ||
|#

## catalog_section

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the catalog section ||
|| **iblockId**
[`catalog_catalog.id`](#catalog_catalog) | Identifier of the information block.

To obtain existing identifiers, you need to use [catalog.catalog.list](./catalog/catalog-catalog-list.md) ||
|| **iblockSectionId**
[`catalog_section.id`](#catalog_section) | Identifier of the parent section.

To obtain existing identifiers, you need to use [catalog.section.list](./section/catalog-section-list.md). 

By default, the top level is selected ||
|| **name**
[`string`](../data-types.md) | Name of the catalog section ||
|| **xmlId**
[`string`](../data-types.md) | External identifier.

Can be used to synchronize the current product section with a similar position in an external system ||
|| **code**
[`string`](../data-types.md) | Code of the catalog section ||
|| **sort**
[`integer`](../data-types.md) | Sorting ||
|| **active**
[`string`](../data-types.md) | Indicator of the activity of the catalog section:
- `Y` — active
- `N` — inactive ||
|| **description**
[`string`](../data-types.md) | Description ||
|| **descriptionType**
[`string`](../data-types.md) | Type of description. Available types: `text`, `html` ||
|#