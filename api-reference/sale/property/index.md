# Order Properties in Online Store: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Order properties are fields that the customer fills out when placing an order: name, phone number, delivery address. Each type of payer will have its own order properties. Field descriptions are in the [sale_order_property](../data-types.md#sale_order_property) reference.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Place an Order on the Website](https://helpdesk.bitrix24.com/open/17300484/)
> 
> User documentation: [Create an order in CRM](https://helpdesk.bitrix24.com/open/8271153)

## Order Property Types

The main property types are set in the `type` field:

- `STRING` — string
- `Y/N` — yes or no
- `NUMBER` — number
- `ENUM` — list of options
- `FILE` — file
- `DATE` — date
- `LOCATION` — location
- `ADDRESS` — address

The `ADDRESS` type is available when the `location` module is installed.

## Special Order Properties

### LOCATION

If you need the customer's location to calculate delivery costs or taxes, create a `LOCATION` type property and enable the required options:

- `isLocation` — to calculate delivery costs
- `isLocation4tax` — to determine tax rates

### STRING

To designate the customer's contact details, create `STRING` type properties and enable the required options:

- `isEmail` — to use as an e-mail
- `isPhone` — to use as a phone number

## Linking Order Properties with Other Objects

**Payer Types.** Determine which type of client the customer belongs to: individual or legal entity. Use the methods [sale.persontype.*](../person-type/index.md) for this.

**Property Groups.** Link the order property to the appropriate group: personal data or delivery data. You can create or modify property groups using the methods [sale.propertygroup.*](../property-group/index.md).

**Property Variants.** If you are creating an ENUM type order property, specify the available options using the methods [sale.propertyvariant.*](../property-variant/index.md). For example, for the property "Delivery Time," specify options like "08:00-12:00," "12:00-16:00," and "16:00-20:00."

**Property Binding.** Set conditions under which the customer will see a specific property. For this, link the order property to the payment system, delivery service, landing page, or trading platform using the methods [sale.propertyRelation.*](../property-relation/index.md). For example, show the "Metro Station" property when "Courier Delivery" is selected.

**Property Values.** View or modify the values of a specific order property using the methods [sale.propertyvalue.*](../property-value/index.md).

## How to Get Started

1. Determine the payer type using [sale.persontype.list](../person-type/sale-person-type-list.md).
2. Retrieve the property group for this payer type using [sale.propertygroup.list](../property-group/sale-property-group-list.md) or create one using [sale.propertygroup.add](../property-group/sale-property-group-add.md). The group ID is required when creating a property.
3. Create an order property using [sale.property.add](./sale-property-add.md).
4. If the property has a fixed list of values, add variants using [sale.propertyvariant.*](../property-variant/index.md).
5. If the property should depend on a payer type, delivery service, or payment system, configure bindings using [sale.propertyRelation.*](../property-relation/index.md).

## Overview of Methods {#all-methods}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the methods: retrieve properties and the field description — store manager, add, update, and delete properties — administrator

#|
|| **Method** | **Description** ||
|| [sale.property.add](./sale-property-add.md) | Adds an order property ||
|| [sale.property.update](./sale-property-update.md) | Updates the fields of an order property ||
|| [sale.property.get](./sale-property-get.md) | Returns the fields of an order property by its identifier ||
|| [sale.property.list](./sale-property-list.md) | Returns a list of order properties ||
|| [sale.property.delete](./sale-property-delete.md) | Deletes an order property ||
|| [sale.property.getFieldsByType](./sale-property-get-fields-by-type.md) | Returns the fields and settings of an order property for a specific property type ||
|#