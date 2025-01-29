# Shipping Property Values in the Online Store: Overview of Methods

When creating a [shipping property](../shipment-property/index.md), you can immediately set values. In an order, there are three books that need to be shipped to different addresses. Create a shipping property called "Delivery Address" with three values. If the delivery address changes, update the shipping property value using the `sale.shipmentpropertyvalue.*` methods.

> Quick navigation: [all methods](#all-methods)

## Linking Shipping Property Values to Other Objects

**Shipments.** Specify the shipment ID. You can obtain a list of IDs using the [sale.shipment.list](../shipment/sale-shipment-list.md) method.

**Shipping Properties.** Create shipping properties using the [sale.shipmentproperty.*](../shipment-property/index.md) methods.

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
|| [sale.shipmentpropertyvalue.getfields](./sale-shipment-property-value-get-fields.md) | Returns available fields for shipping property values ||
|#