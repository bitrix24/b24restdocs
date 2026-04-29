# Trade Catalog: Overview of Sections

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The Trade Catalog in Bitrix24 serves as a unified catalog of products and services for CRM and online stores. It helps maintain up-to-date information on items, expedites document generation in CRM, and automates data exchange with external systems.

The sections of the Trade Catalog cover essential data and settings:

- products, services, and variations
- prices, price types, markups, and rounding rules
- product properties and their values
- catalog structure and product images
- units of measurement, coefficients, and VAT rates
- warehouses, stock levels, and inventory documents

> Quick navigation: [all catalog sections](#all-methods)
>
> User documentation: [Product and Service Catalog: What It Is and How to Work with It](https://helpdesk.bitrix24.com/open/25829591/)

## How to Choose a Section

#|
|| **If you need** | **Open the section** ||
|| To work with products, services, and variations | [Products](./product/index.md) ||
|| To create and modify prices, price types, and rounding rules | [Price](./price/index.md), [Price Types](./price-type/index.md), [Rounding Rules](./rounding-rule/index.md) ||
|| To configure product properties, list values, and display parameters | [Product and Variation Properties](./product-property/index.md), [List Property Values](./product-property-enum/index.md), [Product and Variation Property Parameters](./product-property-feature/index.md), [Section Settings for Properties](./product-property-section/index.md) ||
|| To work with product and variation images | [Product and Variation Images](./product-image/index.md) ||
|| To manage warehouses, stock levels, and accounting documents | [Warehouses](./store/index.md), [Stock Levels by Warehouse](./store-product/index.md), [Inventory Accounting](./document/index.md) ||
|| To obtain reference values from the catalog | [Enumerations](./enum/index.md), [Units of Measurement](./measure/index.md), [VAT Rates](./vat/index.md), [Markups](./extra/index.md) ||
|#

{% note tip "User Documentation" %}

- [How to Create a Product in the Catalog](https://helpdesk.bitrix24.com/open/25792447/)
- [How to Work with Product Variations](https://helpdesk.bitrix24.com/open/18839102/)
- [How to Work with Product Properties](https://helpdesk.bitrix24.com/open/13413370/)
- [Services in CRM](https://helpdesk.bitrix24.com/open/16680094/)

{% endnote %}

## How to Get Started

1. Check the structure of the required object, field names, and their types on the page [Data Types and Object Structure in the Catalog REST API](./data-types.md).
2. Identify the main object of the scenario: product, price, property, warehouse, or accounting document.
3. Open the relevant section and retrieve the list of objects using the `list` method to gather working identifiers.
4. Modify the object using the `add` or `update` methods of the appropriate section.
5. Verify the result using the `get` method and, if necessary, clarify available fields using the `getFields` method.

## Access Permissions

By default, only the Bitrix24 administrator can edit the catalog and configure access permissions. The administrator can also grant this permission to other employees.

{% note tip "User Documentation" %}

- [How to Configure Access Permissions for the Product Catalog](https://helpdesk.bitrix24.com/open/25386568/)

{% endnote %}

## Relationships with Other Objects

**CRM.** Catalog data is used in the product lines of deals, invoices, and estimates: the product ID, price, discount, and VAT are transmitted. For scenarios involving vendors, use the methods from the section [Working with CRM Vendors](./documentcontractor/index.md).

**Online Store.** The catalog generates data for the storefront and product detail pages of the online store.

**Files.** In the catalog methods, files and images are transmitted in the `fileData` or `fileContent` format: file name and content in Base64. The upload format and examples are provided on the page [How to Upload Files](../files/how-to-upload-files.md).

## Overview of Catalog Sections {#all-methods}

> Scope: [`catalog`](../scopes/permissions.md)
>
> Who can perform the method: depending on the method

### Main Catalog Objects

#|
|| **Section** | **Description** ||
|| [Trade Catalog](./catalog/index.md) | Basic methods for working with the trade catalog ||
|| [Products](./product/index.md) | Methods for working with products ||
|| [Products with Variations: Parent Products](./product/sku/index.md) | Methods for working with parent products ||
|| [Products with Variations: Variations](./product/offer/index.md) | Methods for working with product variations ||
|| [Services](./product/service/index.md) | Methods for working with services ||
|| [Catalog Sections](./section/index.md) | Methods for working with catalog sections ||
|| [Product and Variation Images](./product-image/index.md) | Methods for working with product and variation images ||
|#

### Prices and Pricing

#|
|| **Section** | **Description** ||
|| [Price](./price/index.md) | Methods for working with prices ||
|| [Price Types](./price-type/index.md) | Methods for working with price types ||
|| [Binding Price Types to Customer Groups](./price-type/price-type-group/index.md) | Methods for binding price types to customer groups ||
|| [Translations of Price Type Names](./price-type/price-type-lang/index.md) | Methods for translating price type names ||
|| [Rounding Rules](./rounding-rule/index.md) | Methods for working with rounding rules ||
|| [Markups](./extra/index.md) | Methods for working with markups ||
|#

### Product Properties and Parameters

#|
|| **Section** | **Description** ||
|| [Product and Variation Properties](./product-property/index.md) | Methods for working with product and variation properties ||
|| [List Property Values](./product-property-enum/index.md) | Methods for working with list property values ||
|| [Product and Variation Property Parameters](./product-property-feature/index.md) | Methods for working with product and variation property parameters ||
|| [Section Settings for Properties](./product-property-section/index.md) | Methods for working with section settings for properties ||
|#

### Reference and Utility Sections

#|
|| **Section** | **Description** ||
|| [Enumerations](./enum/index.md) | Reference values used in catalog methods ||
|| [Units of Measurement](./measure/index.md) | Methods for working with units of measurement ||
|| [Unit of Measurement Coefficient](./ratio/index.md) | Methods for working with unit of measurement coefficients ||
|| [VAT Rates](./vat/index.md) | Methods for working with VAT rates ||
|#

### Warehouses and Accounting Documents

#|
|| **Section** | **Description** ||
|| [Warehouses](./store/index.md) | Methods for working with warehouses ||
|| [Stock Levels by Warehouse](./store-product/index.md) | Methods for working with stock levels by warehouse ||
|| [Inventory Accounting](./document/index.md) | Methods for working with inventory accounting documents ||
|| [Products in Inventory Accounting Documents](./document/document-element/index.md) | Methods for working with product lines in accounting documents ||
|| [Custom Fields in Inventory Accounting Documents](./userfield-document/index.md) | Methods for working with custom fields in accounting documents ||
|| [Working with CRM Vendors](./documentcontractor/index.md) | Methods for working with vendors in inventory accounting documents ||
|#