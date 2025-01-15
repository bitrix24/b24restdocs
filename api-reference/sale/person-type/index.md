# Types of Payers in an Online Store: Overview of Methods

Customers are divided into individual and legal entities based on payer types. Each payer type has its own properties for placing an order. Depending on the selection, the user sees the corresponding fields, available payment systems, and delivery services.

> Quick navigation: [all methods](#all-methods)

## Connection of Payer Types with Other Objects

In all connections of the online store, the required parameter `personTypeId` is used — the code for the payer type field. You can obtain a list of such codes using the method [sale.persontype.list](./sale-person-type-list.md).

**Correspondence to Individual and Legal Entities.** When creating a payer type, you can specify any name, but you need to define the status: individual or legal entity. To do this, use the methods [sale.businessValuePersonDomain.*](../business-value-person-domain/index.md).

**Order.** Create or modify an order using the methods [sale.order.*](../order/index.md).

**Order Properties.** Create fields that the buyer must fill out when placing an order, such as "Metro Station" or "Delivery Date and Time." To create order properties, use the methods [sale.property.*](../property/index.md).

**Shipment Properties.** If there are multiple shipments in one order, create shipment properties using the methods [sale.shipmentproperty.*](../shipment-property/index.md). For example, if there are three books in an order that need to be sent to different addresses, create shipment properties to specify the address for each shipment.

**Property Groups.** Link the order or shipment property to the appropriate group: personal data, delivery information, company data, contact information. You can create or modify property groups using the methods [sale.propertygroup.*](../property-group/index.md).

**Payment Systems.** Create or modify a payment system using the methods [sale.paysystem.*](../../pay-system/index.md).

## Overview of Methods {#all-methods}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can perform the methods: administrator

#| 
|| **Method** | **Description** ||
|| [sale.persontype.add](./sale-person-type-add.md) | Adds a payer type ||
|| [sale.persontype.update](./sale-person-type-update.md) | Modifies a payer type ||
|| [sale.persontype.get](./sale-person-type-get.md) | Returns the fields of the payer type by identifier ||
|| [sale.persontype.list](./sale-person-type-list.md) | Returns a list of payer types ||
|| [sale.persontype.delete](./sale-person-type-delete.md) | Deletes a payer type ||
|| [sale.persontype.getFields](./sale-person-type-get-fields.md) | Returns the fields of the payer type ||
|#