# Shipping Property Values in the Online Store: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

When creating a [shipping property](../shipment-property/index.md), you can immediately set values. In an order, there are three books that need to be shipped to different addresses. Create a shipping property called "Delivery Address" with three values. If the delivery address changes, update the shipping property value using the `sale.shipmentpropertyvalue.*` methods.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Online Store: Personal Account](https://helpdesk.bitrix24.com/open/26006727/)

## Linking Shipping Property Values to Other Objects

**Shipments.** Specify the shipment ID. You can obtain a list of IDs using the [sale.shipment.list](../shipment/sale-shipment-list.md) method.

**Shipping Properties.** Create shipping properties using the [sale.shipmentproperty.*](../shipment-property/index.md) methods.

## How to Get Started

1. Retrieve the shipment ID using [sale.shipment.list](../shipment/sale-shipment-list.md).
2. Retrieve the shipment property ID using [sale.shipmentproperty.list](../shipment-property/sale-shipment-property-list.md).
3. Change property values using [sale.shipmentpropertyvalue.modify](./sale-shipment-property-value-modify.md).
4. Check values using [sale.shipmentpropertyvalue.get](./sale-shipment-property-value-get.md) or [sale.shipmentpropertyvalue.list](./sale-shipment-property-value-list.md).

## Overview of Methods {#all-methods}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the methods: administrator

#| 
|| **Method** | **Description** ||
|| [sale.shipmentpropertyvalue.modify](./sale-shipment-property-value-modify.md) | Updates the shipping property values ||
|| [sale.shipmentpropertyvalue.get](./sale-shipment-property-value-get.md) | Returns the shipping property values ||
|| [sale.shipmentpropertyvalue.list](./sale-shipment-property-value-list.md) | Returns a list of shipping property values ||
|| [sale.shipmentpropertyvalue.delete](./sale-shipment-propertyvalue-delete.md) | Deletes shipping property values ||
|| [sale.shipmentpropertyvalue.getFields](./sale-shipment-property-value-get-fields.md) | Returns available fields for shipping property values ||
|#