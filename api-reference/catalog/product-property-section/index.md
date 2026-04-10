# Section Settings for Product Catalog: Overview of Methods

This section contains methods for reading and modifying section settings for product properties and variations. These settings define the behavior of the property in the smart filter: whether to display the property, how to display it, and what hint to show.

> Quick Navigation: [All Methods](#all-methods)
>
> User Documentation: [Product Properties in the Bitrix24 Catalog](https://helpdesk.bitrix24.com/open/13413370/)

## Relationship with Other Objects

**Product or Variation Property.** Section settings are set for a property by `propertyId`. The property identifier can be obtained using the methods [catalog.productProperty.list](../product-property/catalog-product-property-list.md) and [catalog.productProperty.get](../product-property/catalog-product-property-get.md).

**Catalog Sections.** The settings are associated with sections of the product catalog and are returned by the object [catalog_product_property_section](../data-types.md#catalog_product_property_section). The `sectionId` field in this object contains the section identifier. A value of `0` is returned for general settings.

**Smart Filter.** The fields of the section settings determine how the property is displayed in the catalog filter:

- `smartFilter` — whether to show the property in the filter,
- `displayType` — how to display the property: checkboxes, radio buttons, or a dropdown list,
- `displayExpanded` — whether to expand the property block in the filter,
- `filterHint` — what hint to show the user.

## Working with Section Settings

1. Obtain the `propertyId` of the desired property.
2. Retrieve the current settings using [catalog.productPropertySection.get](./catalog-product-property-section-get.md) or [catalog.productPropertySection.list](./catalog-product-property-section-list.md).
3. Set new values using the method [catalog.productPropertySection.set](./catalog-product-property-section-set.md).
4. Recheck the result using the method [catalog.productPropertySection.get](./catalog-product-property-section-get.md).

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View Product Catalog" access permission

#| 
|| **Method** | **Description** ||
|| [catalog.productPropertySection.set](./catalog-product-property-section-set.md) | Sets the section settings for a product or variation property ||
|| [catalog.productPropertySection.get](./catalog-product-property-section-get.md) | Returns the section settings for a property by identifier ||
|| [catalog.productPropertySection.list](./catalog-product-property-section-list.md) | Returns a list of section settings for properties based on a filter ||
|#