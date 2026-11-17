# Product Property and Variation Parameters: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Property parameters control how a specific product property or variation is used in the catalog.

The parameter object includes:

- `id` — identifier of the property parameter
- `propertyId` — identifier of the property to which the parameter belongs
- `moduleId` — identifier of the module to which the parameter belongs
- `featureId` — code of the property parameter
- `isEnabled` — indicator of the parameter's activity

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Create and Configure Product Properties in CRM](https://helpdesk.bitrix24.com/open/25882983/)

## Types of Property Parameters

The set of parameters depends on the specific property. The current list for the selected `propertyId` is returned by the method [catalog.productPropertyFeature.getAvailableFeaturesByProperty](./catalog-product-property-feature-get-available-features-by-property.md).

Examples of `moduleId + featureId` combinations:

- `moduleId = iblock` and `featureId = LIST_PAGE_SHOW` — display the property on the item list page
- `moduleId = iblock` and `featureId = DETAIL_PAGE_SHOW` — display the property on the item detail page

## Considerations Before Calling Methods

- To read property parameters, you need the "View product catalog" access permission. To create and update, you additionally need the permission to modify the information block property

- The combination of `propertyId + moduleId + featureId` must be unique. If the record already exists, the method [catalog.productPropertyFeature.add](./catalog-product-property-feature-add.md) will return the error `Internal server error.`

- The [catalog.productPropertyFeature.update](./catalog-product-property-feature-update.md) method looks up the parameter by `propertyId`, `moduleId`, and `featureId`, not by `id`, and requires all four fields

- The methods do not check that `moduleId` and `featureId` are in the list of available parameters of the property

- There is no method to delete a parameter. Parameters are deleted together with the property

## How to Work with Property Parameters

1. Obtain the `propertyId` of the property using the method [catalog.productProperty.list](../product-property/catalog-product-property-list.md)
2. Get the available codes for `moduleId` and `featureId` for the property using the method [catalog.productPropertyFeature.getAvailableFeaturesByProperty](./catalog-product-property-feature-get-available-features-by-property.md)
3. Retrieve the current parameters using the method [catalog.productPropertyFeature.list](./catalog-product-property-feature-list.md) with a filter by `propertyId`
4. If the parameter already exists, modify it using the method [catalog.productPropertyFeature.update](./catalog-product-property-feature-update.md). If the parameter does not exist, add it using the method [catalog.productPropertyFeature.add](./catalog-product-property-feature-add.md)
5. Verify the parameter by `id` using the method [catalog.productPropertyFeature.get](./catalog-product-property-feature-get.md)
6. If necessary, obtain the structure of fields through [catalog.productPropertyFeature.getFields](./catalog-product-property-feature-get-fields.md)

## Relationship of Parameters with Other Objects

**Product or Variation Property.** The parameter is linked to the property via `propertyId`. Manage product or variation properties using the methods [catalog.productProperty.*](../product-property/index.md).

## Parameters in REST 3.0

In [REST 3.0](../../rest-v3.md), property parameters have their own methods. Methods available in both API versions are marked `v 3.0` in the table. Two methods return the field descriptions: [catalog.product.property.feature.field.list](./catalog-product-property-feature-field-list.md) returns all fields, like [catalog.productPropertyFeature.getFields](./catalog-product-property-feature-get-fields.md), and [catalog.product.property.feature.field.get](./catalog-product-property-feature-field-get.md) returns one field by name.

Specifics of the new version methods:

- the [catalog.product.property.feature.update](./catalog-product-property-feature-update-rest-v3.md) method finds a parameter by `id` and changes only `isEnabled`. The `propertyId`, `moduleId`, and `featureId` fields cannot be changed
- the `isEnabled` flag is passed and returned as `true` or `false`, not `Y` or `N`
- if the property already has the parameter, the [catalog.product.property.feature.add](./catalog-product-property-feature-add-rest-v3.md) method returns the `The feature is already assigned to the property` error
- the [catalog.product.property.feature.available.list](./catalog-product-property-feature-available-list.md) method with a filter by `propertyId` returns the parameters available to the property. The [catalog.product.property.feature.available.field.list](./catalog-product-property-feature-available-field-list.md) and [catalog.product.property.feature.available.field.get](./catalog-product-property-feature-available-field-get.md) methods return the description of its fields
- the new version has no method for deleting a parameter either. To make a parameter stop working, disable it: `isEnabled: false`

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the methods:
> - a user with the "View product catalog" permission — to retrieve parameters, available parameters, and field descriptions
> - a user with the "View product catalog" permission and permission to modify the information block of the property — to add and update parameters
> - any user — for the field descriptions in REST 3.0: `catalog.product.property.feature.field.*` and `catalog.product.property.feature.available.field.*`

#| 
|| **Method** | **Description** ||
|| [catalog.productPropertyFeature.add](./catalog-product-property-feature-add.md) | Adds a product or variation property parameter ||
|| [catalog.product.property.feature.add](./catalog-product-property-feature-add-rest-v3.md) | Adds a product or variation property parameter v 3.0 ||
|| [catalog.productPropertyFeature.update](./catalog-product-property-feature-update.md) | Modifies a product or variation property parameter ||
|| [catalog.product.property.feature.update](./catalog-product-property-feature-update-rest-v3.md) | Modifies a product or variation property parameter v 3.0 ||
|| [catalog.productPropertyFeature.get](./catalog-product-property-feature-get.md) | Returns the property parameter by identifier ||
|| [catalog.product.property.feature.get](./catalog-product-property-feature-get-rest-v3.md) | Returns the property parameter by identifier v 3.0 ||
|| [catalog.productPropertyFeature.list](./catalog-product-property-feature-list.md) | Returns a list of product and variation property parameters ||
|| [catalog.product.property.feature.list](./catalog-product-property-feature-list-rest-v3.md) | Returns a list of product and variation property parameters v 3.0 ||
|| [catalog.productPropertyFeature.getAvailableFeaturesByProperty](./catalog-product-property-feature-get-available-features-by-property.md) | Returns available parameters for the specified property ||
|| [catalog.product.property.feature.available.list](./catalog-product-property-feature-available-list.md) | Returns available parameters for the specified property v 3.0 ||
|| [catalog.productPropertyFeature.getFields](./catalog-product-property-feature-get-fields.md) | Returns the description of the property parameter fields ||
|| [catalog.product.property.feature.field.list](./catalog-product-property-feature-field-list.md) | Returns the description of the property parameter fields v 3.0 ||
|| [catalog.product.property.feature.field.get](./catalog-product-property-feature-field-get.md) | Returns the description of a property parameter field ||
|| [catalog.product.property.feature.available.field.list](./catalog-product-property-feature-available-field-list.md) | Returns the description of the available property parameter fields ||
|| [catalog.product.property.feature.available.field.get](./catalog-product-property-feature-available-field-get.md) | Returns the description of an available property parameter field ||
|#
