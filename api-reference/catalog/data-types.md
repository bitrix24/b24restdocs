# Data Types and Object Structure in the REST API Catalog

Basic data types are listed in a separate [article](../data-types.md).

In this article, we will explore the data types and object structure specific to the CRM Catalog.

## Data Types

#|
|| **Type** | **Descriptions and Values** ||
|| [`catalog_catalog.id`](#catalog_catalog) | Integer identifier of the trade catalog (e.g., `1`). You can obtain the identifiers of trade catalogs using the [catalog.catalog.list](./catalog/catalog-catalog-list.md) method ||
|| [`catalog_document.id`](#catalog_document) | Integer identifier of the inventory document, for example `1`. You can obtain the identifiers of documents using the [catalog.document.list](./document/catalog-document-list.md) method ||
|| [`catalog_document_element.id`](#catalog_document_element) | Integer identifier of the product in the inventory document, for example `1`. You can obtain the identifiers of products in documents using the [catalog.document.element.list](./document/document-element/catalog-document-element-list.md) method ||
|| [`catalog_documentcontractor.id`](#catalog_documentcontractor) | Integer identifier of the supplier binding to the inventory document, for example `1`. You can obtain the identifiers of bindings using the [catalog.documentcontractor.list](./documentcontractor/catalog-documentcontractor-list.md) method ||
|| [`catalog_product.id`](#catalog_product) | Integer identifier of the product (e.g., `1`). You can obtain the identifiers of products using the [catalog.product.list](./product/catalog-product-list.md) method ||
|| [`catalog_product_property.id`](#catalog_product_property) | Integer identifier of the product property or variation (e.g., `1`). You can obtain the identifiers of properties using the [catalog.productProperty.list](./product-property/catalog-product-property-list.md) method ||
|| [`catalog_product_sku.id`](#catalog_product_sku) | Integer identifier of the parent product (e.g., `1`). You can obtain the identifiers of parent products using the [catalog.product.sku.list](./product/sku/catalog-product-sku-list.md) method ||
|| [`catalog_product_offer.id`](#catalog_product_offer) | Integer identifier of the product variation (e.g., `1`). You can obtain the identifiers of product variations using the [catalog.product.offer.list](./product/offer/catalog-product-offer-list.md) method ||
|| [`catalog_product_service.id`](#catalog_product_service) | Integer identifier of the service (e.g., `1`). You can obtain the identifiers of services using the [catalog.product.service.list](./product/service/catalog-product-service-list.md) method ||
|| [`catalog_product_image.id`](#catalog_product_image) | Integer identifier of the product image (e.g., `1`). You can obtain the identifiers of product images using the [catalog.productImage.list](./product-image/catalog-product-image-list.md) method ||
|| [`catalog_store.id`](#catalog_store) | Integer identifier of the warehouse (e.g., `1`). You can obtain the identifiers of warehouses using the [catalog.store.list](./store/catalog-store-list.md) method ||
|| [`catalog_measure.id`](#catalog_measure) | Integer identifier of the unit of measurement (e.g., `1`). You can obtain the identifiers of units of measurement using the [catalog.measure.list](./measure/catalog-measure-list.md) method ||
|| [`catalog_ratio.id`](#catalog_ratio) | Integer identifier of the unit of measurement ratio (e.g., `1`). You can obtain the identifiers of unit ratios using the [catalog.ratio.list](./ratio/catalog-ratio-list.md) method ||
|| [`catalog_price.id`](#catalog_price) | Integer identifier of the price, for example `1`. You can obtain the identifiers of prices using the [catalog.price.list](./price/catalog-price-list.md) method ||
|| [`catalog_price_type.id`](#catalog_price_type) | Integer identifier of the price type (e.g., `1`). You can obtain the identifiers of price types using the [catalog.priceType.list](./price-type/catalog-price-type-list.md) method ||
|| [`catalog_price_type_lang.id`](#catalog_price_type_lang) | Integer identifier of the translation of price type names (e.g., `1`). You can obtain the identifiers of translations using the [catalog.priceTypeLang.list](./price-type/price-type-lang/catalog-price-type-lang-list.md) method ||
|| [`catalog_language.lid`](#catalog_language) | String identifier of the language, consisting of two characters (e.g., `de`). You can obtain the identifiers of languages using the [catalog.priceTypeLang.getLanguages](./price-type/price-type-lang/catalog-price-type-lang-get-languages.md) method ||
|| [`catalog_rounding_rule.id`](#catalog_rounding_rule) | Integer identifier of the price rounding rule (e.g., `1`). You can obtain the identifiers of price rounding rules using the [catalog.roundingRule.list](./rounding-rule/catalog-rounding-rule-list.md) method ||
|| [`catalog_extra.id`](#catalog_extra) | Integer identifier of the markup (e.g., `1`). You can obtain the identifiers of markups using the [catalog.extra.list](./extra/catalog-extra-list.md) method ||
|| [`catalog_section.id`](#catalog_section) | Integer identifier of the catalog section (e.g., `1`). You can obtain the identifiers of catalog sections using the [catalog.section.list](./section/catalog-section-list.md) method ||
|| [`catalog_storeproduct.id`](#catalog_storeproduct) | Integer identifier of the record of product stock in the warehouse, for example `1`. You can obtain the identifiers using the [catalog.storeproduct.list](./store-product/catalog-store-product-list.md) method ||
|| [`catalog_vat.id`](#catalog_vat) | Integer identifier of the VAT rate (e.g., `1`). You can obtain the identifiers of VAT rates using the [catalog.vat.list](./vat/catalog-vat-list.md) method ||
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
[`integer`](../data-types.md) | Identifier of the parent information block of the trade catalog. Filled only for trade catalogs of variations.

To obtain existing identifiers of information blocks, you need to use [catalog.catalog.list](./catalog/catalog-catalog-list.md)
||
|| **skuPropertyId**
[`integer`](../data-types.md) | Identifier of the property that stores the identifier of the parent product. Filled only for trade catalogs of variations.

To obtain existing identifiers of properties, you need to use [catalog.productProperty.list](./product-property/catalog-product-property-list.md)
||
|| **subscription**
[`string`](../data-types.md) | Is content sold? Possible values:
- `Y` — yes
- `N` — no

This parameter is used only in the on-premise version
||
|| **vatId**
[`catalog_vat.id`](#catalog_vat) | Identifier of VAT.

To obtain existing identifiers of VAT, you need to use [catalog.vat.list](./vat/catalog-vat-list.md)
||
|#

### catalog_document

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the inventory document ||
|| **docType**
[`string`](../data-types.md) | Type of document. Available types can be obtained using the [catalog.enum.getStoreDocumentTypes](./enum/catalog-enum-get-store-document-types.md) method ||
|| **docNumber**
[`string`](../data-types.md) | Internal document number. If not provided, it is generated automatically ||
|| **title**
[`string`](../data-types.md) | Title of the document ||
|| **siteId**
[`string`](../data-types.md) | Site code to which the document relates. Default is `s1` ||
|| **responsibleId**
[`integer`](../data-types.md) | Identifier of the responsible person ||
|| **createdBy**
[`integer`](../data-types.md) | Identifier of the user who created the document ||
|| **modifiedBy**
[`integer`](../data-types.md) | Identifier of the user who modified the document ||
|| **status**
[`string`](../data-types.md) | Document status:
- `N` — draft,
- `Y` — approved,
- `C` — canceled.
  
The value is automatically changed when the document is approved or canceled ||
|| **statusBy**
[`integer`](../data-types.md) | User who changed the document status ||
|| **dateStatus**
[`datetime`](../data-types.md) | Date of status change ||
|| **dateCreate**
[`datetime`](../data-types.md) | Date of document creation ||
|| **dateModify**
[`datetime`](../data-types.md) | Date of last modification of the document ||
|| **dateDocument**
[`datetime`](../data-types.md) | Date of document approval ||
|| **currency**
[`string`](../data-types.md) | Currency of the document ||
|| **total**
[`double`](../data-types.md) | Total amount for the products in the document. The value is calculated automatically after approval but can be set manually ||
|| **commentary**
[`string`](../data-types.md) | Commentary for the document ||
|#

### catalog_userfield_document

#|
|| **Name**
`type` | **Description** ||
|| **documentId**
[`catalog_document.id`](#catalog_document) | Identifier of the inventory document ||
|| **documentType**
[`string`](../data-types.md) | Type of inventory document ||
|| **fieldN**
[`mixed`](../data-types.md) | Value of the custom field of the document, where `N` — identifier of the custom field, for example `field7097` ||
|#

### catalog_document_element

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the product in the inventory document ||
|| **docId**
[`catalog_document.id`](#catalog_document) | Identifier of the inventory document ||
|| **elementId**
[`catalog_product.id`](#catalog_product) | Identifier of the catalog product ||
|| **storeFrom**
[`catalog_store.id`](#catalog_store) | Identifier of the source warehouse. Used for documents where write-off is required ||
|| **storeTo**
[`catalog_store.id`](#catalog_store) | Identifier of the recipient warehouse. Used for incoming and transfer documents ||
|| **amount**
[`double`](../data-types.md) | Quantity of the product ||
|| **purchasingPrice**
[`double`](../data-types.md) | Purchasing price ||
|#

### catalog_documentcontractor

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the supplier binding to the inventory document ||
|| **documentId**
[`catalog_document.id`](#catalog_document) | Identifier of the inventory document to which the supplier is bound ||
|| **entityTypeId**
[`integer`](../data-types.md) | Identifier of the CRM object type:
`3` — contact 
`4` — company  ||
|| **entityId**
[`integer`](../data-types.md) | Identifier of the supplier: contact or company ||
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
[`string`](../data-types.md) | Activity indicator. Possible values:
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
[`string`](../data-types.md) | Is purchase of the product allowed when it is out of stock? Possible values:
- `Y` — yes
- `N` — no 
||
|| **createdBy**
[`user.id`](../data-types.md) | Created by ||
|| **modifiedBy**
[`user.id`](../data-types.md) | Modified by ||
|| **dateActiveFrom**
[`datetime`](../data-types.md) | Date of activity start ||
|| **dateActiveTo**
[`datetime`](../data-types.md) | Date of activity end ||
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
[`catalog_vat.id`](#catalog_vat) | Identifier of VAT ||
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

The list of currencies can be obtained using the [crm.currency.list](../crm/currency/crm-currency-list.md) method.

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
[`object\|array`](../data-types.md) | Value of the property of the main product, where `N` — identifier of the property. There can be multiple properties. 

The value is specified in the format `{valueId: valueId, value: value}` or in the format `[{valueId: valueId1, value: value1}, ..., {valueId: valueIdN, value: valueN}]`, if the property is multiple. Here `valueId` — identifier of the property value, and `value` — value of the property. 

If `valueId` is not specified, the existing value will be removed from the database and replaced with the new one specified in `value`. If the property is multiple, all existing values of the property for which `valueId` was not specified will be removed.

`valueId` of all properties of the product can be obtained using the [catalog.product.get](./product/catalog-product-get.md) and [catalog.product.list](./product/catalog-product-list.md) methods
||
|#

### catalog_product_property

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the property ||
|| **timestampX**
[`datetime`](../data-types.md) | Date and time of property modification ||
|| **iblockId**
[`catalog_catalog.id`](#catalog_catalog) | Identifier of the trade catalog ||
|| **name**
[`string`](../data-types.md) | Name of the property ||
|| **active**
[`char`](../data-types.md) | Activity indicator. Possible values:
- `Y` — yes
- `N` — no ||
|| **sort**
[`integer`](../data-types.md) | Sorting index ||
|| **code**
[`string`](../data-types.md) | Symbolic code of the property ||
|| **defaultValue**
[`text`](../data-types.md) | Default value of the property ||
|| **propertyType**
[`string`](../data-types.md) | Basic type of the property. Allowed values:
- `N` — number
- `S` — string
- `L` — list
- `F` — file
- `E` — binding to elements
- `G` — binding to sections ||
|| **userType**
[`string`](../data-types.md) | User type of the property. The value corresponds to the specified `propertyType`.

Examples of values:
- `DateTime` — date and time
- `Money` — monetary value with currency
- `SKU` — binding to product variations
- `directory` — binding to directory
- `employee` — binding to employee
- `UserID` — binding to user
- `EList` — selection of an element from a list
- `EAutocomplete` — binding to elements with auto-search
- `SectionAuto` — binding to sections with auto-search
- `HTML` — value in HTML format
- `map_google` — coordinates and address on Google map
- `DiskFile` — binding to a file from Bitrix24.Drive
- `ECrm` — binding to CRM elements
- `BoolEnum` — checkbox based on a list ||
|| **rowCount**
[`integer`](../data-types.md) | Number of rows in the input field ||
|| **colCount**
[`integer`](../data-types.md) | Number of columns in the input field ||
|| **listType**
[`char`](../data-types.md) | Appearance of the list ||
|| **multiple**
[`char`](../data-types.md) | Indicator of multiplicity. Possible values:
- `Y` — yes
- `N` — no ||
|| **xmlId**
[`string`](../data-types.md) | External identifier of the property ||
|| **fileType**
[`string`](../data-types.md) | Allowed extensions for the file type property ||
|| **multipleCnt**
[`integer`](../data-types.md) | Number of input fields for multiple values ||
|| **linkIblockId**
[`catalog_catalog.id`](#catalog_catalog) | Identifier of the linked information block. 

Available identifiers can be obtained using the [catalog.catalog.list](./catalog/catalog-catalog-list.md) method ||
|| **withDescription**
[`char`](../data-types.md) | Indicator of storing the description of the value ||
|| **searchable**
[`char`](../data-types.md) | Indicator of participation in search. Possible values:
- `Y` — yes
- `N` — no ||
|| **filtrable**
[`char`](../data-types.md) | Indicator of participation in filtering ||
|| **isRequired**
[`char`](../data-types.md) | Indicator of mandatory status. Possible values:
- `Y` — yes
- `N` — no ||
|| **hint**
[`string`](../data-types.md) | Hint for the field ||
|| **userTypeSettings**
[`object`](../data-types.md) | Object with settings for the user type ||
|#

### catalog_product_property_enum

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the value of the list property ||
|| **propertyId**
[`catalog_product_property.id`](#catalog_product_property) | Identifier of the product property or variation ||
|| **value**
[`string`](../data-types.md) | Value of the list element ||
|| **xmlId**
[`string`](../data-types.md) | External identifier of the value of the list ||
|| **def**
[`char`](../data-types.md) | Indicator of the default value. Possible values:
- `Y` — default
- `N` — not default ||
|| **sort**
[`integer`](../data-types.md) | Sorting index ||
|#

### catalog_product_property_features

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the property feature ||
|| **propertyId**
[`catalog_product_property.id`](#catalog_product_property) | Identifier of the product property or variation ||
|| **moduleId**
[`string`](../data-types.md) | Identifier of the module to which the property feature belongs ||
|| **featureId**
[`string`](../data-types.md) | Code of the property feature ||
|| **isEnabled**
[`char`](../data-types.md) | Indicator of the activity of the property feature. Possible values:
- `Y` — enabled
- `N` — disabled ||
|#

### catalog_product_property_section

#|
|| **Value**
`type` | **Description** ||
|| **iblockId**
[`catalog_catalog.id`](#catalog_catalog) | Identifier of the trade catalog ||
|| **propertyId**
[`catalog_product_property.id`](#catalog_product_property) | Identifier of the product property or variation ||
|| **sectionId**
[`integer`](../data-types.md) | Identifier of the catalog section.

For general section settings, the value `0` is returned ||
|| **smartFilter**
[`char`](../data-types.md) | Show the property in the smart filter. Possible values:
- `Y` — yes
- `N` — no ||
|| **displayType**
[`char`](../data-types.md) | Appearance of the property in the smart filter. Possible values:
- `F` — checkboxes
- `K` — radio buttons
- `P` — dropdown list ||
|| **displayExpanded**
[`char`](../data-types.md) | Show the property expanded in the filter. Possible values:
- `Y` — yes
- `N` — no ||
|| **filterHint**
[`string`](../data-types.md) | Hint in the smart filter for visitors ||
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
[`string`](../data-types.md) | Activity indicator. Possible values:
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
[`string`](../data-types.md) | Is purchase of the parent product allowed when it is out of stock? Possible values:
- `Y` — yes
- `N` — no 

For parent products, the ability to edit this field is available only in the on-premise version when the option "Show the Trade Catalog tab for products with trade offers" is enabled
||
|| **createdBy**
[`user.id`](../data-types.md) | Created by ||
|| **modifiedBy**
[`user.id`](../data-types.md) | Modified by ||
|| **dateActiveFrom**
[`datetime`](../data-types.md) | Date of activity start ||
|| **dateActiveTo**
[`datetime`](../data-types.md) | Date of activity end ||
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
[`catalog_vat.id`](#catalog_vat) | Identifier of VAT.

For parent products, the ability to edit this field is available only in the on-premise version when the option "Show the Trade Catalog tab for products with trade offers" is enabled ||
|| **vatIncluded**
[`string`](../data-types.md) | Is VAT included in the price? Possible values:
- `Y` — yes
- `N` — no

For parent products, the ability to edit this field is available only in the on-premise version when the option "Show the Trade Catalog tab for products with trade offers" is enabled
||
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
- `3` — parent product with variations
- `6` — parent product without variations
||
|| **purchasingCurrency**
[`string`](../data-types.md) | Currency of the purchasing price.

The list of currencies can be obtained using the [crm.currency.list](../crm/currency/crm-currency-list.md) method.

Not editable when inventory accounting is enabled.

For parent products, the ability to edit this field is available only in the on-premise version when the option "Show the Trade Catalog tab for products with trade offers" is enabled
||
|| **purchasingPrice**
[`float`](../data-types.md) | Purchasing price.

Not editable when inventory accounting is enabled.

For parent products, the ability to edit this field is available only in the on-premise version when the option "Show the Trade Catalog tab for products with trade offers" is enabled
||
|| **quantity**
[`float`](../data-types.md) | Quantity.

Not editable when inventory accounting is enabled.

For parent products, the ability to edit this field is available only in the on-premise version when the option "Show the Trade Catalog tab for products with trade offers" is enabled
||
|| **propertyN**
[`object\|array`](../data-types.md) | Value of the property of the parent product, where `N` — identifier of the property. There can be multiple properties. 

The value is specified in the format `{valueId: valueId, value: value}` or in the format `[{valueId: valueId1, value: value1}, ..., {valueId: valueIdN, value: valueN}]`, if the property is multiple. Here `valueId` — identifier of the property value, and `value` — value of the property. 

If `valueId` is not specified, the existing value will be removed from the database and replaced with the new one specified in `value`. If the property is multiple, all existing values of the property for which `valueId` was not specified will be removed.

`valueId` of all properties of the parent product can be obtained using the [catalog.product.sku.get](./product/sku/catalog-product-sku-get.md) and [catalog.product.sku.list](./product/sku/catalog-product-sku-list.md) methods
||
|#

### catalog_product_offer

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the product variation ||
|| **iblockId**
[`catalog_catalog.id`](#catalog_catalog) | Identifier of the information block of the trade catalog.

To obtain existing identifiers of information blocks, you need to use [catalog.catalog.list](./catalog/catalog-catalog-list.md)
||
|| **name**
[`string`](../data-types.md) | Name of the product variation ||
|| **active**
[`string`](../data-types.md) | Activity indicator. Possible values:
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
[`string`](../data-types.md) | Is purchase of the product variation allowed when it is out of stock? Possible values:
- `Y` — yes
- `N` — no 
||
|| **createdBy**
[`user.id`](../data-types.md) | Created by ||
|| **modifiedBy**
[`user.id`](../data-types.md) | Modified by ||
|| **dateActiveFrom**
[`datetime`](../data-types.md) | Date of activity start ||
|| **dateActiveTo**
[`datetime`](../data-types.md) | Date of activity end ||
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
[`string`](../data-types.md) | Permission to subscribe to the product variation. Possible values:
- `Y` — yes
- `N` — no
- `D` — default
||
|| **vatId**
[`catalog_vat.id`](#catalog_vat) | Identifier of VAT ||
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
- `4` — variation
- `5` — variation without product
||
|| **purchasingCurrency**
[`string`](../data-types.md) | Currency of the purchasing price.

The list of currencies can be obtained using the [crm.currency.list](../crm/currency/crm-currency-list.md) method.

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
[`object\|array`](../data-types.md) | Value of the property of the product variation, where `N` — identifier of the property. There can be multiple properties. 

The value is specified in the format `{valueId: valueId, value: value}` or in the format `[{valueId: valueId1, value: value1}, ..., {valueId: valueIdN, value: valueN}]`, if the property is multiple. Here `valueId` — identifier of the property value, and `value` — value of the property. 

If `valueId` is not specified, the existing value will be removed from the database and replaced with the new one specified in `value`. If the property is multiple, all existing values of the property for which `valueId` was not specified will be removed.

`valueId` of all properties of the trade offer can be obtained using the [catalog.product.offer.get](./product/offer/catalog-product-offer-get.md) and [catalog.product.offer.list](./product/offer/catalog-product-offer-list.md) methods
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
[`string`](../data-types.md) | Activity indicator. Possible values:
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
[`datetime`](../data-types.md) | Date of activity start ||
|| **dateActiveTo**
[`datetime`](../data-types.md) | Date of activity end ||
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
|| **vatId**
[`catalog_vat.id`](#catalog_vat) | Identifier of VAT ||
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
[`object\|array`](../data-types.md) | Value of the property of the service, where `N` — identifier of the property. There can be multiple properties. 

The value is specified in the format `{valueId: valueId, value: value}` or in the format `[{valueId: valueId1, value: value1}, ..., {valueId: valueIdN, value: valueN}]`, if the property is multiple. Here `valueId` — identifier of the property value, and `value` — value of the property. 

If `valueId` is not specified, the existing value will be removed from the database and replaced with the new one specified in `value`. If the property is multiple, all existing values of the property for which `valueId` was not specified will be removed.

`valueId` of all properties of the service can be obtained using the [catalog.product.service.get](./product/service/catalog-product-service-get.md) and [catalog.product.service.list](./product/service/catalog-product-service-list.md) methods
||
|#

### catalog_product_image

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the image ||
|| **name**
[`string`](../data-types.md) | Name of the image ||
|| **productId**
[`integer`](../data-types.md) | Identifier of the product ||
|| **type**
[`string`](../data-types.md) | Type of image:
- `DETAIL_PICTURE` — detailed picture, field available in the old product card
- `PREVIEW_PICTURE` — picture for the announcement, field available in the old product card
- `MORE_PHOTO` — picture
||
|| **createTime**
[`datetime`](../data-types.md) | Date of image creation ||
|| **downloadUrl**
[`string`](../data-types.md) | Download link, signed with the current access token ||
|| **detailUrl**
[`string`](../data-types.md) | Link to the image ||
|#

### catalog_store

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the warehouse ||
|| **address**
[`string`](../data-types.md) | Address of the warehouse ||
|| **title**
[`string`](../data-types.md) | Name of the warehouse ||
|| **active**
[`string`](../data-types.md) | Activity. Possible values:
- `Y` — yes
- `N` — no ||
|| **description**
[`string`](../data-types.md) | Description ||
|| **gpsN**
[`double`](../data-types.md) | GPS latitude ||
|| **gpsS**
[`double`](../data-types.md) | GPS longitude ||
|| **imageId**
[`object`](../data-types.md) | Image. Object in the format `{fileData: [value1, value2]}`, where:
- `value1` – name of the picture file with extension
- `value2` – picture in base64 format

To delete the picture, use the object in the format `{remove: ‘Y’}` ||
|| **dateModify**
[`datetime`](../data-types.md) | Date of modification ||
|| **dateCreate**
[`datetime`](../data-types.md) | Date of creation ||
|| **userId**
[`user.id`](../data-types.md) | Created by ||
|| **modifiedBy**
[`user.id`](../data-types.md) | Modified by ||
|| **phone**
[`string`](../data-types.md) | Phone ||
|| **schedule**
[`string`](../data-types.md) | Working hours ||
|| **xmlId**
[`string`](../data-types.md) | External code.

Can be used to synchronize the current warehouse with a similar position in an external system ||
|| **sort**
[`integer`](../data-types.md) | Sorting ||
|| **email**
[`string`](../data-types.md) | E-mail ||
|| **issuingCenter**
[`string`](../data-types.md) | Is it a pickup point? Possible values:
- `Y` – yes
- `N` – no ||
|| **code**
[`string`](../data-types.md) | Symbolic code ||
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

Only one unit of measurement from the entire directory can take the value `Y`
||
|| **measureTitle**
[`string`](../data-types.md) | Title of the unit of measurement ||
|| **symbol**
[`string`](../data-types.md) | Conditional designation ||
|| **symbolIntl**
[`string`](../data-types.md) | International conditional designation ||
|| **symbolLetterIntl**
[`string`](../data-types.md) | International code letter designation ||
|#

### catalog_ratio

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the unit of measurement ratio ||
|| **productId**
[`integer`](../data-types.md) | Identifier of the product ||
|| **ratio**
[`double`](../data-types.md) | Ratio of the unit of measurement ||
|| **isDefault**
[`string`](../data-types.md) | Is this unit of measurement ratio the default ratio? Possible values:
- `Y` — yes
- `N` — no
||
|#

### catalog_price

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the price ||
|| **productId**
[`catalog_product.id`](#catalog_product) | Identifier of the product ||
|| **catalogGroupId**
[`catalog_price_type.id`](#catalog_price_type) | Identifier of the price type ||
|| **price**
[`double`](../data-types.md) | Value of the price ||
|| **priceScale**
[`double`](../data-types.md) | Value of the base price ||
|| **currency**
[`string`](../data-types.md) | Currency of the price ||
|| **quantityFrom**
[`double`](../data-types.md) | Minimum quantity for applying the price. Deprecated parameter ||
|| **quantityTo**
[`double`](../data-types.md) | Maximum quantity for applying the price. Deprecated parameter  ||
|| **extraId**
[`catalog_extra.id`](#catalog_extra) | Identifier of the markup. Deprecated parameter  ||
|| **timestampX**
[`datetime`](../data-types.md) | Date of modification ||
|#

### catalog_price_type

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the price type ||
|| **name**
[`string`](../data-types.md) | Code of the price type ||
|| **base**
[`string`](../data-types.md) | Is the price type basic? Possible values:
- `Y` — yes
- `N` — no
||
|| **sort**
[`integer`](../data-types.md) | Sorting ||
|| **xmlId**
[`string`](../data-types.md) | External code.

Can be used to synchronize the current price type with a similar position in an external system ||
|| **timestampX**
[`datetime`](../data-types.md) | Date of modification ||
|| **createdBy**
[`user.id`](../data-types.md) | Created by ||
|| **modifiedBy**
[`user.id`](../data-types.md) | Modified by ||
|| **dateCreate**
[`datetime`](../data-types.md) | Date of creation ||
|#

### catalog_price_type_group

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the binding of the price type to the customer group ||
|| **catalogGroupId**
[`catalog_price_type.id`](#catalog_price_type) | Identifier of the price type ||
|| **groupId**
[`integer`](../data-types.md) | Identifier of the customer group ||
|| **access**
[`char`](../data-types.md) | Type of access. Possible values:
- `Y` — right to purchase at this price type
- `N` — right to view this price type ||
|#

### catalog_enum

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) \| [`string`](../data-types.md) | Identifier of the enumeration element ||
|| **name**
[`string`](../data-types.md) | Name of the enumeration element ||
|#

### catalog_price_type_lang

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the translation of the price type name ||
|| **catalogGroupId**
[`catalog_price_type.id`](#catalog_price_type) | Identifier of the price type ||
|| **name**
[`string`](../data-types.md) | Translation of the price type name ||
|| **lang**
[`catalog_language.lid`](#catalog_language) | Identifier of the language ||
|#

### catalog_language

#|
|| **Value**
`type` | **Description** ||
|| **lid**
[`string`](../data-types.md) | Identifier of the language ||
|| **name**
[`string`](../data-types.md) | Name of the language ||
|| **active**
[`string`](../data-types.md) | Activity indicator. Possible values:
- `Y` — yes
- `N` — no
||
|#

### catalog_rounding_rule

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the price rounding rule ||
|| **catalogGroupId**
[`catalog_price_type.id`](#catalog_price_type) | Price type ||
|| **price**
[`double`](../data-types.md) | Minimum price for rounding ||
|| **roundType**
[`integer`](../data-types.md) | Type of rounding. Possible values:
- `1` — mathematical rounding
- `2` — rounding up (in favor of the store)
- `4` — rounding down (in favor of the customer)
||
|| **roundPrecision**
[`double`](../data-types.md) | Rounding precision ||
|| **createdBy**
[`user.id`](../data-types.md) | Created by ||
|| **modifiedBy**
[`user.id`](../data-types.md) | Modified by ||
|| **dateCreate**
[`datetime`](../data-types.md) | Date of creation ||
|| **dateModify**
[`datetime`](../data-types.md) | Date of modification ||
|#

### catalog_extra

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the markup ||
|| **name**
[`string`](../data-types.md) | Name of the markup ||
|| **percentage**
[`double`](../data-types.md) | Amount of the markup ||
|#

### catalog_section

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

Can be used to synchronize the current catalog section with a similar position in an external system ||
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

### catalog_storeproduct

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the stock record ||
|| **productId**
[`catalog_product.id`](#catalog_product) | Identifier of the product ||
|| **storeId**
[`catalog_store.id`](#catalog_store) | Identifier of the warehouse ||
|| **amount**
[`double`](../data-types.md) | Available quantity of the product ||
|| **quantityReserved**
[`double`](../data-types.md) | Quantity of the product in reserve ||
|#

### catalog_vat

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the VAT rate ||
|| **name**
[`string`](../data-types.md) | Name of the VAT rate ||
|| **active**
[`string`](../data-types.md) | Indicator of the activity of the VAT rate. Possible values:
- `Y` — active
- `N` — inactive
||
|| **rate**
[`double`](../data-types.md) | Amount of the VAT rate ||
|| **sort**
[`integer`](../data-types.md) | Sorting ||
|| **timestampX**
[`datetime`](../data-types.md) | Time of the last modification ||
|#

## Objects Used in Responses

### rest_field_description {#rest_field_description}

#|
|| **Value**
`type` | **Description** ||
|| **isImmutable**
[`boolean`](../data-types.md) | Indicator of the ability to change the field value after creation. 

If this indicator is set for the field, then when creating the object, you can specify the field value, but it cannot be changed during updates ||
|| **isReadOnly**
[`boolean`](../data-types.md) | Indicator of "read-only". 

If this indicator is set for the field, then in the operations of adding and updating the object, the field value does not need to be passed. The value is generated automatically and is intended for read-only ||
|| **isRequired**
[`boolean`](../data-types.md) | Indicator of the mandatory field for add or update operations ||
|| **type**
[`string`](../data-types.md) | Data type of the field values. Possible values: 
- `integer`
- `double`
- `string`
- `char`
- `list`
- `text`
- `file`
- `date`
- `datetime`
- `datatype`
- `productpropertysettings`
||
|#
