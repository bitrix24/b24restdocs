# Property Groups in the Online Store: Overview of Methods

All order or shipment properties are grouped into categories: personal information, delivery details, company information, and contact information.

Each payer type has its own groups. The payer type of a created property group cannot be changed.

> Quick Navigation: [all methods](#all-methods)

## Linking Property Groups to Other Objects

**Payer Types.** Specify the payer type to which you want to associate the order property group. You can retrieve the available payer types using the [sale.persontype.list](../person-type/sale-person-type-list.md) method.

**Order Properties.** Create fields that the buyer must fill out when placing an order, such as "Metro Station" or "Delivery Date and Time." Use the [sale.property.*](../property/index.md) methods to create order properties.

**Shipment Properties.** If there are multiple shipments in one order, create shipment properties using the [sale.shipmentproperty.*](../shipment-property/index.md) methods. For example, if an order contains three books that need to be sent to different addresses, create shipment properties to specify the address for each shipment.

## Overview of Methods {#all-methods}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the methods: administrator

#| 
|| **Method** | **Description** ||
|| [sale.propertygroup.add](./sale-property-group-add.md) | Creates a property group ||
|| [sale.propertygroup.update](./sale-property-group-update.md) | Updates the fields of a property group ||
|| [sale.propertygroup.get](./sale-property-group-get.md) | Returns the values of all fields in the property group ||
|| [sale.propertygroup.list](./sale-property-group-list.md) | Returns a list of property groups ||
|| [sale.propertygroup.delete](./sale-property-group-delete.md) | Deletes a property group ||
|| [sale.propertygroup.getFields](./sale-property-group-get-fields.md) | Returns the available fields of property groups ||
|#