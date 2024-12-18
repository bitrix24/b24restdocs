# Shipping Properties in the Online Store: Overview of Methods

Shipping properties are additional parameters that can be set for the shipment of goods: delivery address and method, shipment date. For example, in an order with three books that need to be sent to different addresses. To specify the address for each shipment, create shipping properties.

> Quick navigation: [all methods](#all-methods)

## Connection of Shipping Properties with Other Objects

**Payer Types.** Determine what type of client the buyer is: individual or legal entity. Use the methods [sale.persontype.*](../person-type/index.md) for this.

**Property Groups.** Associate the shipping property with the appropriate group: personal data, delivery information, company data, contact information. You can create or modify property groups using the methods [sale.propertygroup.*](../property-group/index.md).

**Shipping Property Values.** Change the values of a specific shipment property using the methods [sale.shipmentpropertyvalue.*](../shipment-property-value/index.md).

## Overview of Methods {#all-methods}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can perform the methods: administrator

#|
|| **Method** | **Description** ||
|| [sale.shipmentproperty.add](./sale-shipment-property-add.md) | Adds a shipping property ||
|| [sale.shipmentproperty.update](./sale-shipment-property-update.md) | Updates the fields of the shipping property ||
|| [sale.shipmentproperty.get](./sale-shipment-property-get.md) | Returns the values of the shipping property fields by identifier ||
|| [sale.shipmentproperty.list](./sale-shipment-property-list.md) | Returns a list of shipping properties ||
|| [sale.shipmentproperty.delete](./sale-shipment-property-delete.md) | Deletes a shipping property ||
|| [sale.shipmentproperty.getFieldsByType](./sale-shipment-property-get-fields-by-type.md) | Returns the fields and settings of the shipping property for a specific property type ||
|#