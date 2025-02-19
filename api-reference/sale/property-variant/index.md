# Order Property Options of ENUM Type in Online Store: Overview of Methods

[Order properties](../property/index.md) can be of various types:
- `STRING` — string,
- `Y/N` — yes/no,
- `NUMBER` — number,
- `ENUM` — enumeration,
- `FILE` — file,
- `DATE` — date,
- `LOCATION` — location,
- `ADDRESS` — address.

For an order property of the "list" type `ENUM`, you need to specify the available options. For example, create an order property "Delivery Time" and add options "08:00-12:00", "12:00-16:00", and "16:00-20:00".

> Quick navigation: [all methods](#all-methods)

## Linking Order Property Options with Other Objects

**Order properties.** Specify the identifier of the order property. You can obtain a list of identifiers using the [sale.property.list](../property/sale-property-list.md) method.

## Overview of Methods {#all-methods}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can perform the methods: administrator

#|
|| **Method** | **Description** ||
|| [sale.propertyvariant.add](./sale-property-variant-add.md) | Adds a property option ||
|| [sale.propertyvariant.update](./sale-property-variant-update.md) | Updates the fields of a property option ||
|| [sale.propertyvariant.get](./sale-property-variant-get.md) | Retrieves a property option by id ||
|| [sale.propertyvariant.list](./sale-property-variant-list.md) | Retrieves a list of property options ||
|| [sale.propertyvariant.delete](./sale-property-variant-delete.md) | Deletes a property option ||
|| [sale.propertyvariant.getFields](./sale-property-variant-get-fields.md) | Returns available fields of a property option ||
|#