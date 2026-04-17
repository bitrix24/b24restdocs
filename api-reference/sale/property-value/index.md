# Order Property Values in an Online Store: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A property value refers to the actual data transmitted in the order field during checkout. For instance, this includes the phone number, delivery address, delivery date, or customer comments. These values are essential for processing a specific order, transmitting data for delivery and payment, and maintaining accurate information in the order detail form.

Each property value is defined by a set of fields:

- `id` — identifier of the property value
- `name` — name of the property
- `orderId` — identifier of the order
- `code` — symbolic code of the property
- `orderPropsId` — identifier of the order property
- `orderPropsXmlId` — external identifier of the order property
- `value` — value of the order property

> Quick Navigation: [all methods](#all-methods)

## Considerations Before Calling Methods

- The methods `sale.propertyvalue.*` are accessible only to administrators.
- The `value` field is transmitted in a format that corresponds to the type of order property. For the `FILE` type, use the object [`sale_order_property_value_file_value`](../data-types.md#sale_order_property_value_file_value); for other types, use [`string`](../../data-types.md). The property type can be obtained using the methods [sale.property.get](../property/sale-property-get.md) and [sale.property.list](../property/sale-property-list.md).
- The method [sale.propertyvalue.modify](./sale-property-value-modify.md) accepts a complete set of order property values. If some values are not provided, the current values of those properties will be deleted.

## How to Start Working with Property Values

1. Obtain `orderId` using the method [sale.order.get](../order/sale-order-get.md) or [sale.order.list](../order/sale-order-list.md).
2. Retrieve `orderPropsId` using the method [sale.property.list](../property/sale-property-list.md).
3. Check available fields using the method [sale.propertyvalue.getFields](./sale-property-value-get-fields.md).
4. View current values through [sale.propertyvalue.list](./sale-property-value-list.md) or [sale.propertyvalue.get](./sale-property-value-get.md).
5. Prepare a complete set of order property values and update it using the method [sale.propertyvalue.modify](./sale-property-value-modify.md).
6. If necessary, delete a specific property value using the method [sale.propertyvalue.delete](./sale-property-value-delete.md).

## Relationship with Other Objects

**Order.** Property values are always tied to a specific order through `orderId`. To work with an order, use the methods in the [sale.order.*](../order/index.md) section.

**Order Properties.** A value pertains to a specific property via `orderPropsId`. To work with order properties, use the methods in the [sale.property.*](../property/index.md) section.

**Property Binding.** The visibility and display conditions of a property during order checkout are configured separately using the methods in the [sale.propertyRelation.*](../property-relation/index.md) section.

## Overview of Methods {#all-methods}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the methods: administrator

#| 
|| **Method** | **Description** ||
|| [sale.propertyvalue.modify](./sale-property-value-modify.md) | Modifies the property value ||
|| [sale.propertyvalue.get](./sale-property-value-get.md) | Retrieves the property value by identifier ||
|| [sale.propertyvalue.list](./sale-property-value-list.md) | Retrieves a list of property values ||
|| [sale.propertyvalue.delete](./sale-property-value-delete.md) | Deletes the property value ||
|| [sale.propertyvalue.getFields](./sale-property-value-get-fields.md) | Returns available fields for the property value ||
|#