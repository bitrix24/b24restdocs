# Values of List Properties for Products: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A property of type `List` has a set of permissible values. Each value is stored as a separate element:

- `id` — identifier of the list value
- `propertyId` — identifier of the property to which the value belongs
- `value` — textual value of the list element
- `xmlId` — external code of the list value
- `def` — indicator of the default value
- `sort` — sorting index

> Quick navigation: [all methods](#all-methods)

## Considerations Before Calling Methods

- Methods only work for properties of type `List`. If a property of another type is specified when adding a value, the method [catalog.productPropertyEnum.add](./catalog-product-property-enum-add.md) will return the error `Only list properties are supported`.

- The `xmlId` field is mandatory and must be unique within a single property.

- To read list property values and delete them, you need the "View product catalog" access permission. For creating and updating, you additionally need permission to modify the information block property.

## How to Work with List Property Values

1. Check the structure of the fields using [catalog.productPropertyEnum.getFields](./catalog-product-property-enum-get-fields.md).
2. Determine the `propertyId` of the list property using the method [catalog.productProperty.list](../product-property/catalog-product-property-list.md).
3. Request the current list values using the method [catalog.productPropertyEnum.list](./catalog-product-property-enum-list.md) with a filter by `propertyId`.
4. Add a value using the method [catalog.productPropertyEnum.add](./catalog-product-property-enum-add.md) or modify an existing one using the method [catalog.productPropertyEnum.update](./catalog-product-property-enum-update.md).
5. Retrieve a value by `id` using the method [catalog.productPropertyEnum.get](./catalog-product-property-enum-get.md) or delete it using the method [catalog.productPropertyEnum.delete](./catalog-product-property-enum-delete.md).

## Relationship with Other Objects

**Product or Variation Property.** The list value is associated with the property through `propertyId`. To work with properties, use the methods in the [catalog.productProperty.*](../product-property/index.md) section.

**Product Card.** If a product or variation has a property of type `List`, the `id` of the required value is passed in the field of that property.

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

#| 
|| **Method** | **Description** ||
|| [catalog.productPropertyEnum.add](./catalog-product-property-enum-add.md) | Adds a list property value ||
|| [catalog.productPropertyEnum.update](./catalog-product-property-enum-update.md) | Modifies a list property value ||
|| [catalog.productPropertyEnum.get](./catalog-product-property-enum-get.md) | Returns a list property value by identifier ||
|| [catalog.productPropertyEnum.list](./catalog-product-property-enum-list.md) | Returns a list of list property values by filter ||
|| [catalog.productPropertyEnum.delete](./catalog-product-property-enum-delete.md) | Deletes a list property value ||
|| [catalog.productPropertyEnum.getFields](./catalog-product-property-enum-get-fields.md) | Returns the description of the fields of a list property value ||
|#