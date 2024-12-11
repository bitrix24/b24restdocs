# Order Properties in Online Store: Overview of Methods

Order properties are fields that the customer fills out when placing an order: name, phone number, delivery address. Each type of payer will have its own order properties.

> Quick navigation: [all methods](#all-methods)
> 
> User documentation: [Create an order in CRM](https://helpdesk.bitrix24.com/open/8271153)

## Special Order Properties

### LOCATION

Make sure to create LOCATION type properties for each type of payer. Allow the use of property values as the customer's location for the following parameters:
- `isLocation` — to calculate delivery costs,
- `isLocation4tax` — to determine tax rates.

### STRING

To transmit customer data to payment systems and cash registers, create STRING type properties and mark one of the options:
- `isEmail` — to use as an e-mail,
- `isPhone` — to use as a phone number.

## Linking Order Properties with Other Objects

**Payer Types.** Determine which type of client the customer belongs to: individual or legal entity. Use the methods [sale.persontype.*](../person-type/index.md) for this.

**Property Groups.** Link the order property to the appropriate group: personal data or delivery data. You can create or modify property groups using the methods [sale.propertygroup.*](../property-group/index.md).

**Property Variants.** If you are creating an ENUM type order property, specify the available options using the methods [sale.propertyvariant.*](../property-variant/index.md). For example, for the property "Delivery Time," specify options like "08:00-12:00," "12:00-16:00," and "16:00-20:00."

**Property Binding.** Set conditions under which the customer will see a specific property. For this, link the order property to the payment system, delivery service, landing page, or trading platform using the methods [sale.propertyRelation.*](../property-relation/index.md). For example, if the customer selects "Courier Delivery," they must specify the order property "Metro Station."

**Property Values.** View or modify the values of a specific order property using the methods [sale.propertyvalue.*](../property-value/index.md).

## Overview of Methods {#all-methods}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the methods: administrator

#|
|| **Method** | **Description** ||
|| [sale.property.add](./sale-property-add.md) | Adds an order property ||
|| [sale.property.update](./sale-property-update.md) | Updates the fields of an order property ||
|| [sale.property.get](./sale-property-get.md) | Returns the value of an order property by its identifier ||
|| [sale.property.list](./sale-property-list.md) | Returns a list of order properties ||
|| [sale.property.delete](./sale-property-delete.md) | Deletes an order property ||
|| [sale.property.getFieldsByType](./sale-property-get-fields-by-type.md) | Returns the fields and settings of an order property for a specific property type ||
|#