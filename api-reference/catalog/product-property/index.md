# Product and Variation Properties: Overview of Methods

Products and variations have properties such as color, size, material, brand, and others. These properties help customers find and select products on the site.

In Bitrix24, properties can be configured.

- Make them common for all products. Such properties are available for all products in the catalog; for example, the "Brand" property can be specified for all products.

- Bind them to product sections. For instance, the "Material" property may only be available for the "Clothing" section, while the "Diagonal" property may only be available for the "Televisions" section.

> Quick navigation: [all methods and events](#all-methods) 
>
> User documentation: [Product and Variation Properties](https://helpdesk.bitrix24.com/open/13413370/)

## Property Types

Each property has a type: String, List, Number, Employee Binding, and others. A complete list of property types is described in the `fields` parameter of the [catalog.productProperty.add](./catalog-product-property-add.md) method. To work with values of the List type properties, you can use the [catalog.productPropertyEnum.*](../product-property-enum/index.md) methods.

{% note warning "" %}

The type is set when creating the property and cannot be changed.

{% endnote %}

## Linking Product Properties with Other Objects

**Trade Catalog.** Product properties must be linked to a specific trade catalog. You can obtain the identifiers of available trade catalogs using the [catalog.catalog.list](../catalog/catalog-catalog-list.md) method.

**Currencies.** In the Money type property, you specify the amount and currency. You can work with currencies through the [crm.currency.*](../../crm/currency/index.md) methods.

**User.** In the product or variation property, you can set a binding to an employee. You can obtain the user identifier using the [user.get](../../user/user-get.md) and [user.search](../../user/user-search.md) methods.

**CRM.** The Binding to CRM Elements type property links products with CRM objects: [leads](../../crm/leads/index.md), [deals](../../crm/deals/index.md), [SPAs](../../crm/universal/index.md), [contacts](../../crm/contacts/index.md), and [companies](../../crm/companies/index.md). In products, this property is displayed as a link to the CRM object.

**Universal Lists.** Products can be linked to list elements through the Binding to Elements property in list form. You can work with lists using the [list.*](../../lists/index.md) methods.

**Files.** The File type property allows you to upload and store files in the product. Files do not receive identifiers in the Drive module, so Drive methods are not applicable. For more details, read the article [How to Upload Files](../../files/how-to-upload-files.md).

{% note tip "User Documentation" %}

- [Product Properties](https://helpdesk.bitrix24.com/open/21302554/)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [catalog.productProperty.add](./catalog-product-property-add.md) | Adds a property for products or variations ||
|| [catalog.productProperty.delete](./catalog-product-property-delete.md) | Deletes a property for products or variations ||
|| [catalog.productProperty.get](./catalog-product-property-get.md) | Returns the values of the product or variation property fields ||
|| [catalog.productProperty.getFields](./catalog-product-property-get-fields.md) | Returns the fields of the product or variation properties ||
|| [catalog.productProperty.list](./catalog-product-property-list.md) | Returns a list of properties for products or variations ||
|| [catalog.productProperty.update](./catalog-product-property-update.md) | Updates the fields of the product or variation properties ||
|#