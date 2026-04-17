# Binding Order Properties in an Online Store: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Binding a property determines in which scenario the order field will be displayed to the customer during checkout. You can link properties to payment systems, delivery services, landing pages, and trading platforms.

Each binding is defined by a set of fields:

- `entityType` — type of the binding object 
- `entityId` — identifier of the selected object type
- `propertyId` — identifier of the order property

> Quick navigation: [all methods](#all-methods)

## How to Get Started with Property Binding

1. Obtain the `propertyId` using the [sale.property.list](../property/sale-property-list.md) method.
2. Define the object type in `entityType`:
   - `P` — payment system
   - `D` — delivery
   - `L` — landing page
   - `T` — trading platform
3. Find the object identifier in `entityId` for the selected type.
4. Check the available binding fields using the [sale.propertyRelation.getFields](./sale-property-relation-get-fields.md) method.
5. Create the binding using the [sale.propertyRelation.add](./sale-property-relation-add.md) method.
6. Verify the current bindings with the [sale.propertyRelation.list](./sale-property-relation-list.md) method.
7. Remove unnecessary bindings using the [sale.propertyRelation.deleteByFilter](./sale-property-relation-delete-by-filter.md) method.

## Linking to Other Objects

**Order Properties.** The binding applies to a specific order property via the `propertyId` field. Working with order properties is done in the [sale.property.*](../property/index.md) section.

**Payment Systems.** If `entityType` is `P`, the payment system identifier is passed in `entityId`. You can obtain it using the [sale.paysystem.list](../../pay-system/sale-pay-system-list.md) method.

**Delivery Services.** For `entityType = D`, specify the delivery service identifier in `entityId`, which is returned by the [sale.delivery.getlist](../delivery/delivery/sale-delivery-get-list.md) method.

**Order Sources.** When the type is `T`, the trading platform identifier is used in `entityId`. A list of order sources is available through the [sale.tradePlatform.list](../trade-platform/sale-trade-platform-list.md) method.

**Landing Pages.** For binding with `entityType = L`, the landing page identifier is required in `entityId`. You can obtain the identifier using the [landing.landing.getList](../../landing/page/methods/landing-landing-get-list.md) method.

## Overview of Methods {#all-methods}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the methods: administrator

#| 
|| **Method** | **Description** ||
|| [sale.propertyRelation.add](./sale-property-relation-add.md) | Adds a property binding ||
|| [sale.propertyRelation.list](./sale-property-relation-list.md) | Retrieves a list of property bindings ||
|| [sale.propertyRelation.deleteByFilter](./sale-property-relation-delete-by-filter.md) | Deletes a property binding ||
|| [sale.propertyRelation.getFields](./sale-property-relation-get-fields.md) | Returns the fields of the property binding ||
|#