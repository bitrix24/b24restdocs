# Order Property Options of ENUM Type in Online Store: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A value option is a list item in an order property of the `ENUM` type. For example, the "Delivery Time" property can have the options "08:00-12:00", "12:00-16:00", and "16:00-20:00". Other property types are described in the [Order Properties](../property/index.md) section.

The `sale.propertyvariant.*` methods create, update, read, and delete options.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Create and Configure Product Properties in CRM](https://helpdesk.bitrix24.com/open/25882983/)

## Option Structure

#|
|| **Field** | **Description** ||
|| `id` | Option identifier. Pass it to the `get`, `update`, and `delete` methods ||
|| `orderPropsId` | Order property identifier. Set on creation and cannot be changed afterwards ||
|| `name` | Name displayed to the customer ||
|| `value` | Symbolic code. Stored in the order as the property value ||
|| `sort` | Position in the list, `100` by default ||
|| `description` | Option description ||
|#

## Linking Order Property Options with Other Objects

**Order properties.** An option always belongs to a single property, whose identifier is passed in the `orderPropsId` field. To retrieve properties, use the [sale.property.list](../property/sale-property-list.md) method with the `{"type": "ENUM"}` filter.

**Orders.** An order stores the `value` code of the selected option. If an option is updated or deleted, the property value in existing orders does not change.

## Limitations and Specifics

- Options are used only in properties of the `ENUM` type. The [sale.propertyvariant.add](./sale-property-variant-add.md) method does not check the type and adds an option to a property of any type
- Uniqueness of `value` within a property is not checked
- In [sale.propertyvariant.update](./sale-property-variant-update.md), the `name` and `value` fields are required in every call, and `orderPropsId` cannot be changed

## How to Get Started

1. Create an order property of type `ENUM` using [sale.property.add](../property/sale-property-add.md).
2. Add a property value variant using [sale.propertyvariant.add](./sale-property-variant-add.md).
3. Check variants using [sale.propertyvariant.get](./sale-property-variant-get.md) or [sale.propertyvariant.list](./sale-property-variant-list.md).
4. If necessary, update or delete a variant using [sale.propertyvariant.update](./sale-property-variant-update.md) or [sale.propertyvariant.delete](./sale-property-variant-delete.md).

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