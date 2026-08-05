# Product and Variation Properties: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Products and variations have properties such as color, size, material, brand, and others. These properties help customers find and select products on the site.

Properties can be made common for all products or linked to catalog sections. For example, the "Brand" property can be available for all products, while the "Material" property can be available only in the "Clothing" section.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Product and Variation Properties](https://helpdesk.bitrix24.com/open/13413370/)

## Property Types

Each property has a type: String, List, Number, Employee Binding, and others. A complete list of property types is described in the `fields` parameter of the [catalog.productProperty.add](./catalog-product-property-add.md) method. To work with values of the List type properties, you can use the [catalog.productPropertyEnum.*](../product-property-enum/index.md) methods.

{% note warning "" %}

The type is set when creating the property and cannot be changed.

{% endnote %}

## Relationship with Other Objects

**Trade Catalog.** Product properties must be linked to a specific trade catalog. You can obtain the identifiers of available trade catalogs using the [catalog.catalog.list](../catalog/catalog-catalog-list.md) method.

**Currencies.** In the Money type property, you specify the amount and currency. You can work with currencies through the [crm.currency.*](../../crm/currency/index.md) methods.

**User.** In the product or variation property, you can set a binding to an employee. You can obtain the user identifier using the [user.get](../../user/user-get.md) and [user.search](../../user/user-search.md) methods.

**CRM.** The Binding to CRM Elements type property links products with CRM objects: [leads](../../crm/leads/index.md), [deals](../../crm/deals/index.md), [SPAs](../../crm/universal/index.md), [contacts](../../crm/contacts/index.md), and [companies](../../crm/companies/index.md). In products, this property is displayed as a link to the CRM object.

**Universal Lists.** Products can be linked to list elements through the Binding to Elements property in list form. You can work with lists using the [list.*](../../lists/index.md) methods.

**Files.** The File type property allows you to upload and store files in the product. Files do not receive identifiers in the Drive module, so Drive methods are not applicable. For more details, read the article [How to Upload Files](../../files/how-to-upload-files.md).

{% note tip "User Documentation" %}

- [Product Properties](https://helpdesk.bitrix24.com/open/21302554/)

{% endnote %}

## How to Start

1. Get the trade catalog identifier using [catalog.catalog.list](../catalog/catalog-catalog-list.md)
2. Create a property using [catalog.productProperty.add](./catalog-product-property-add.md)
3. If the property should be available only in specific sections, configure the binding using [catalog.productPropertySection.*](../product-property-section/index.md)
4. If the property has the List type, add values using [catalog.productPropertyEnum.*](../product-property-enum/index.md)
5. Use the property code in product, service, parent product, or variation methods

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

#|
|| **Method** | **Description** ||
|| [catalog.productProperty.add](./catalog-product-property-add.md) | Adds a product or variation property ||
|| [catalog.productProperty.update](./catalog-product-property-update.md) | Updates the fields of a product or variation property ||
|| [catalog.productProperty.get](./catalog-product-property-get.md) | Returns the values of the fields of a product or variation property ||
|| [catalog.productProperty.list](./catalog-product-property-list.md) | Returns a list of product or variation properties ||
|| [catalog.productProperty.delete](./catalog-product-property-delete.md) | Deletes a product or variation property ||
|| [catalog.productProperty.getFields](./catalog-product-property-get-fields.md) | Returns the fields of a product or variation property ||
|#
