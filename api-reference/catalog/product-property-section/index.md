# Section Settings for Product Catalog: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Section settings define how product and variation properties work in trade catalog sections. The settings control how the property is displayed in the smart filter: whether to show the property, how to display it, and what hint to show.

> Quick Navigation: [All Methods](#all-methods)
>
> User Documentation: [Product Properties in the Bitrix24 Catalog](https://helpdesk.bitrix24.com/open/13413370/)

## Relationship of Section Settings with Other Objects

**Product or Variation Property.** Section settings are set for a property by `propertyId`. The property identifier can be obtained using the methods [catalog.productProperty.list](../product-property/catalog-product-property-list.md) and [catalog.productProperty.get](../product-property/catalog-product-property-get.md).

**Catalog Sections.** The settings are associated with sections of the product catalog and are returned by the object [catalog_product_property_section](../data-types.md#catalog_product_property_section). The `sectionId` field in this object contains the section identifier. A value of `0` is returned for general settings.

**Smart Filter.** The fields of the section settings determine how the property is displayed in the catalog filter:

- `smartFilter` — whether to show the property in the filter
- `displayType` — how to display the property, for example as checkboxes, radio buttons, or a dropdown list. The available display types depend on the property type
- `displayExpanded` — whether to expand the property block in the filter
- `filterHint` — what hint to show the user

## How to Start

1. Obtain the `propertyId` of the required property
2. Retrieve the current settings using [catalog.productPropertySection.get](./catalog-product-property-section-get.md) or [catalog.productPropertySection.list](./catalog-product-property-section-list.md)
3. Set new values using [catalog.productPropertySection.set](./catalog-product-property-section-set.md)
4. Recheck the result using [catalog.productPropertySection.get](./catalog-product-property-section-get.md)

## Section Settings in REST 3.0

In [REST 3.0](../../rest-v3.md), section settings have their own methods. They are marked `v 3.0` in the table, except for [catalog.product.property.section.field.list](./catalog-product-property-section-field-list.md) and [catalog.product.property.section.field.get](./catalog-product-property-section-field-get.md): these methods return the field descriptions and exist only in the new version.

Specifics of the new version methods:

- the methods work with the settings of the whole catalog and of a single section. The section is set by the `sectionId` parameter; `0` means the whole catalog
- in [catalog.product.property.section.set](./catalog-product-property-section-set-rest-v3.md) and [catalog.product.property.section.get](./catalog-product-property-section-get-rest-v3.md), the property and the section are passed in the `propertyId` and `sectionId` parameters
- the `smartFilter` and `displayExpanded` flags are passed and returned as `true` or `false`. A disabled flag is returned as `false`, not `null`
- unknown fields in `fields` cause an error instead of being skipped

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the methods:
> - a user with the "View product catalog" permission — to retrieve the settings
> - a user with the "View product catalog" permission and permission to modify the information block of the property — for the `catalog.productPropertySection.set` and `catalog.product.property.section.set` methods
> - any user — for `catalog.product.property.section.field.*`

#|
|| **Method** | **Description** ||
|| [catalog.productPropertySection.set](./catalog-product-property-section-set.md) | Sets the section settings for a product or variation property ||
|| [catalog.product.property.section.set](./catalog-product-property-section-set-rest-v3.md) | Sets the section settings for a product or variation property v 3.0 ||
|| [catalog.productPropertySection.get](./catalog-product-property-section-get.md) | Returns the section settings for a property by identifier ||
|| [catalog.product.property.section.get](./catalog-product-property-section-get-rest-v3.md) | Returns the section settings of a property for the catalog or a section v 3.0 ||
|| [catalog.productPropertySection.list](./catalog-product-property-section-list.md) | Returns a list of section settings for properties based on a filter ||
|| [catalog.product.property.section.list](./catalog-product-property-section-list-rest-v3.md) | Returns a list of section settings for properties based on a filter v 3.0 ||
|| [catalog.product.property.section.field.list](./catalog-product-property-section-field-list.md) | Returns the description of the section property settings fields ||
|| [catalog.product.property.section.field.get](./catalog-product-property-section-field-get.md) | Returns the description of a section property settings field ||
|#
