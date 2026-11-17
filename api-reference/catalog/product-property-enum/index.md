# Values of List Properties for Products: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A property of type `List` has a set of permissible values. Each value is stored as a separate element:

- `id` — identifier of the list value
- `propertyId` — identifier of the property to which the value belongs
- `value` — textual value of the list element
- `xmlId` — external code of the list value
- `def` — indicator of the default value
- `sort` — sorting index

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Create and Configure Product Properties in CRM](https://helpdesk.bitrix24.com/open/25882983/)

## Considerations Before Calling Methods

- Methods only work for properties of type `List`. If a property of another type is specified when adding a value, the method [catalog.productPropertyEnum.add](./catalog-product-property-enum-add.md) will return the error `Only list properties are supported`

- The `xmlId` field is mandatory and must be unique within a single property

- To read values, you need the "View product catalog" permission. To add, update, and delete values, you additionally need permission to modify the information block of the property

- The `catalog.productPropertyEnum.update` method requires `propertyId`, `value`, and `xmlId` on every call. You cannot move a value to another property

## How to Work with List Property Values

1. Check the structure of the fields using [catalog.productPropertyEnum.getFields](./catalog-product-property-enum-get-fields.md)
2. Determine the `propertyId` of the list property using the method [catalog.productProperty.list](../product-property/catalog-product-property-list.md)
3. Request the current list values using the method [catalog.productPropertyEnum.list](./catalog-product-property-enum-list.md) with a filter by `propertyId`
4. Add a value using the method [catalog.productPropertyEnum.add](./catalog-product-property-enum-add.md) or modify an existing one using the method [catalog.productPropertyEnum.update](./catalog-product-property-enum-update.md)
5. Retrieve a value by `id` using the method [catalog.productPropertyEnum.get](./catalog-product-property-enum-get.md) or delete it using the method [catalog.productPropertyEnum.delete](./catalog-product-property-enum-delete.md)

## Relationship with Other Objects

**Product or Variation Property.** The list value is associated with the property through `propertyId`. Manage product or variation properties using the methods [catalog.productProperty.*](../product-property/index.md).

**Product Card.** If a product or variation has a property of type `List`, the `id` of the required value is passed in the field of that property.

## Values in REST 3.0

In [REST 3.0](../../rest-v3.md), list property values have their own methods. Methods available in both API versions are marked `v 3.0` in the table. Two methods return the field descriptions: [catalog.product.property.enum.field.list](./catalog-product-property-enum-field-list.md) returns all fields, like [catalog.productPropertyEnum.getFields](./catalog-product-property-enum-get-fields.md), and [catalog.product.property.enum.field.get](./catalog-product-property-enum-field-get.md) returns one field by name.

Specifics of the new version methods:

- the [catalog.product.property.enum.update](./catalog-product-property-enum-update-rest-v3.md) method changes only the passed fields: you do not need to pass `value` and `xmlId` in every call. A value cannot be moved to another property: if you pass `propertyId`, the method returns an error
- the `def` flag is passed as `true` or `false`, not `Y` or `N`. The methods do not clear the flag on other values of the property, so a property can end up with several default values
- the external ID `xmlId` must be unique among the values of all list properties in Bitrix24, not only within one property
- the [catalog.product.property.enum.list](./catalog-product-property-enum-list-rest-v3.md) method does not accept negation by `propertyId` in the filter and does not sort by `def`

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the methods:
> - a user with the "View product catalog" permission — to retrieve values and field descriptions
> - a user with the "View product catalog" permission and permission to modify the information block of the property — to add, update, and delete values
> - any user — for the field descriptions in REST 3.0, the `catalog.product.property.enum.field.*` methods

#| 
|| **Method** | **Description** ||
|| [catalog.productPropertyEnum.add](./catalog-product-property-enum-add.md) | Adds a list property value ||
|| [catalog.product.property.enum.add](./catalog-product-property-enum-add-rest-v3.md) | Adds a list property value v 3.0 ||
|| [catalog.productPropertyEnum.update](./catalog-product-property-enum-update.md) | Modifies a list property value ||
|| [catalog.product.property.enum.update](./catalog-product-property-enum-update-rest-v3.md) | Modifies a list property value v 3.0 ||
|| [catalog.productPropertyEnum.get](./catalog-product-property-enum-get.md) | Returns a list property value by identifier ||
|| [catalog.product.property.enum.get](./catalog-product-property-enum-get-rest-v3.md) | Returns a list property value by identifier v 3.0 ||
|| [catalog.productPropertyEnum.list](./catalog-product-property-enum-list.md) | Returns a list of list property values by filter ||
|| [catalog.product.property.enum.list](./catalog-product-property-enum-list-rest-v3.md) | Returns a list of list property values by filter v 3.0 ||
|| [catalog.productPropertyEnum.delete](./catalog-product-property-enum-delete.md) | Deletes a list property value ||
|| [catalog.product.property.enum.delete](./catalog-product-property-enum-delete-rest-v3.md) | Deletes a list property value v 3.0 ||
|| [catalog.productPropertyEnum.getFields](./catalog-product-property-enum-get-fields.md) | Returns the description of the fields of a list property value ||
|| [catalog.product.property.enum.field.list](./catalog-product-property-enum-field-list.md) | Returns the description of the fields of a list property value v 3.0 ||
|| [catalog.product.property.enum.field.get](./catalog-product-property-enum-field-get.md) | Returns the description of a list property value field ||
|#
