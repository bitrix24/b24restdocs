# Product and Variation Properties: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Products and variations have properties such as color, size, material, brand, and others. These properties help customers find and select products on the site.

Properties can be made common for all products or linked to catalog sections. For example, the "Brand" property can be available for all products, while the "Material" property can be available only in the "Clothing" section.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Product and Variation Properties](https://helpdesk.bitrix24.com/open/13413370/)

## Property Types

Each property has a type: String, List, Number, Employee Binding, and others. A complete list of property types is described in the `fields` parameter of the [catalog.productProperty.add](./catalog-product-property-add.md) method. To work with values of the List type properties, you can use the [catalog.productPropertyEnum.*](../product-property-enum/index.md) methods.

{% note warning "" %}

You can change the type using the [catalog.productProperty.update](./catalog-product-property-update.md) method, but do this only for a property without values: saved values are not converted to the new type.

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

## Limitations and Specifics

- Properties are created only in a trade catalog. If you pass an information block that is not a catalog, the `catalog.productProperty.add` method returns an error, and `catalog.productProperty.list` returns an empty list
- In `catalog.productProperty.update`, the `iblockId` field is required even if only the name changes
- Deleting a property deletes its values for all products and variations. The deletion cannot be undone
- Property flags are passed as `Y` or `N` strings

## Properties in REST 3.0

In [REST 3.0](../../rest-v3.md), properties have the [catalog.product.property.add](./catalog-product-property-add-rest-v3.md) method for adding, the `catalog.product.property.field.list` and `catalog.product.property.field.get` methods for retrieving the field schema, and the [catalog.product.property.type.list](./catalog-product-property-type-list.md) directory of property types. A property can be retrieved, updated, and deleted with the methods of the previous API version.

Specifics of the adding method:

- the method creates a string property; the property type cannot be passed
- the `multiple` and `isRequired` flags are also passed as `Y` or `N` strings, not `true` or `false`
- only `id` and `controlId` are filled in the response. The field values of the new property can be retrieved using the [catalog.productProperty.get](./catalog-product-property-get.md) method

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the methods:
> - a user with permission to view the catalog — for `catalog.productProperty.get`, `catalog.productProperty.list`, and `catalog.productProperty.getFields`
> - a user with permission to view the catalog and permission to edit the catalog information block — for adding, updating, and deleting properties, including `catalog.product.property.add`
> - any user — for `catalog.product.property.field.*`, `catalog.product.property.type.list`, and `catalog.product.property.type.field.*`

#|
|| **Method** | **Description** ||
|| [catalog.productProperty.add](./catalog-product-property-add.md) | Adds a product or variation property ||
|| [catalog.product.property.add](./catalog-product-property-add-rest-v3.md) | Adds a product or variation property v 3.0 ||
|| [catalog.productProperty.update](./catalog-product-property-update.md) | Updates the fields of a product or variation property ||
|| [catalog.productProperty.get](./catalog-product-property-get.md) | Returns the values of the fields of a product or variation property ||
|| [catalog.productProperty.list](./catalog-product-property-list.md) | Returns a list of product or variation properties ||
|| [catalog.productProperty.delete](./catalog-product-property-delete.md) | Deletes a product or variation property ||
|| [catalog.productProperty.getFields](./catalog-product-property-get-fields.md) | Returns the fields of a product or variation property ||
|| [catalog.product.property.field.list](./catalog-product-property-field-list.md) | Returns a list of product property fields v 3.0 ||
|| [catalog.product.property.field.get](./catalog-product-property-field-get.md) | Returns the description of a product property field ||
|| [catalog.product.property.type.list](./catalog-product-property-type-list.md) | Returns a list of product property types ||
|| [catalog.product.property.type.field.list](./catalog-product-property-type-field-list.md) | Returns a list of product property type fields ||
|| [catalog.product.property.type.field.get](./catalog-product-property-type-field-get.md) | Returns the description of a product property type field ||
|#
