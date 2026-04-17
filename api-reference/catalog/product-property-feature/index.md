# Product Property and Variation Parameters: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Property parameters control how a specific product property or variation is used in the catalog.

The parameter object includes:

- `id` — identifier of the property parameter
- `propertyId` — identifier of the property to which the parameter belongs
- `moduleId` — identifier of the module to which the parameter belongs
- `featureId` — code of the property parameter
- `isEnabled` — indicator of the parameter's activity

> Quick navigation: [all methods](#all-methods)

## Types of Property Parameters

The set of parameters depends on the specific property. The current list for the selected `propertyId` is returned by the method [catalog.productPropertyFeature.getAvailableFeaturesByProperty](./catalog-product-property-feature-get-available-features-by-property.md).

Examples of `moduleId + featureId` combinations:

- `moduleId = iblock` and `featureId = LIST_PAGE_SHOW` — display the property on the item list page
- `moduleId = iblock` and `featureId = DETAIL_PAGE_SHOW` — display the property on the item detail page

## Considerations Before Calling Methods

- To read property parameters, you need the "View product catalog" access permission. To create and update, you additionally need the permission to modify the information block property.

- The combination of `propertyId + moduleId + featureId` must be unique. If the record already exists, the method [catalog.productPropertyFeature.add](./catalog-product-property-feature-add.md) will return the error `Duplicate entry ... for key ...`.

## How to Work with Property Parameters

1. Obtain the `propertyId` of the property using the method [catalog.productProperty.list](../product-property/catalog-product-property-list.md).
2. Get the available codes for `moduleId` and `featureId` for the property using the method [catalog.productPropertyFeature.getAvailableFeaturesByProperty](./catalog-product-property-feature-get-available-features-by-property.md).
3. Retrieve the current parameters using the method [catalog.productPropertyFeature.list](./catalog-product-property-feature-list.md) with a filter by `propertyId`.
4. If the parameter already exists, modify it using the method [catalog.productPropertyFeature.update](./catalog-product-property-feature-update.md). If the parameter does not exist, add it using the method [catalog.productPropertyFeature.add](./catalog-product-property-feature-add.md).
5. Verify the parameter by `id` using the method [catalog.productPropertyFeature.get](./catalog-product-property-feature-get.md).
6. If necessary, obtain the structure of fields through [catalog.productPropertyFeature.getFields](./catalog-product-property-feature-get-fields.md).

## Relationship of Parameters with Other Objects

**Product or Variation Property.** The parameter is linked to the property via `propertyId`. To work with properties, use the methods in the [catalog.productProperty.*](../product-property/index.md) section.

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

#| 
|| **Method** | **Description** ||
|| [catalog.productPropertyFeature.add](./catalog-product-property-feature-add.md) | Adds a product or variation property parameter ||
|| [catalog.productPropertyFeature.update](./catalog-product-property-feature-update.md) | Modifies a product or variation property parameter ||
|| [catalog.productPropertyFeature.get](./catalog-product-property-feature-get.md) | Returns the property parameter by identifier ||
|| [catalog.productPropertyFeature.list](./catalog-product-property-feature-list.md) | Returns a list of product and variation property parameters ||
|| [catalog.productPropertyFeature.getAvailableFeaturesByProperty](./catalog-product-property-feature-get-available-features-by-property.md) | Returns available parameters for the specified property ||
|| [catalog.productPropertyFeature.getFields](./catalog-product-property-feature-get-fields.md) | Returns the description of the property parameter fields ||
|#