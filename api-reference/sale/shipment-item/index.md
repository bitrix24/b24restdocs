# Shipment Table Section in the Online Store: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The shipment table section is the list of products included in an order shipment. Each item links a basket item to a shipment and sets the product quantity in the `quantity` field. The product name and price are stored in the basket item, not in the table section item.

The `sale.shipmentitem.*` methods add, update, retrieve, and delete shipment table section items. The item fields are described in the [sale_order_shipment_item](../data-types.md#sale_order_shipment_item) type.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Delivery Services](https://helpdesk.bitrix24.com/open/17297482/)

## Connection of the Shipment Table Section with Other Objects

**Shipment.** An item is linked to a shipment through the `orderDeliveryId` field. The shipment identifier can be retrieved using the [sale.shipment.list](../shipment/sale-shipment-list.md) method.

**Order.** The shipment and the basket item must belong to the same order. A list of orders can be retrieved using the [sale.order.list](../order/sale-order-list.md) method.

**Basket.** An item refers to a basket item through the `basketId` field. The basket item identifier can be retrieved using the [sale.basketitem.list](../basket-item/sale-basket-item-list.md) method.

## How Product Quantity Is Distributed {#quantity}

The product quantity of a basket item is distributed across the order shipments. Product not added to any shipment is held in the system shipment. The [sale.shipment.*](../shipment/index.md) methods do not return the system shipment, while [sale.shipmentitem.list](./sale-shipment-item-list.md) returns its items as well. For an item of the system shipment, the [sale.shipmentitem.get](./sale-shipment-item-get.md) method returns an empty array.

- The total `quantity` across all shipments cannot exceed the quantity in the basket item
- A basket item can be included in a shipment only once
- `basketId` and `orderDeliveryId` cannot be changed after the item is added. To move a product to another shipment, delete the item and add a new one
- Items of the system shipment cannot be changed or deleted
- In a shipment with `deducted` = `Y`, you cannot add or delete products or change the quantity; only `xmlId` can be changed

## How to Get Started

1. Retrieve the shipment identifier using the [sale.shipment.list](../shipment/sale-shipment-list.md) method. If there is no shipment yet, create one using the [sale.shipment.add](../shipment/sale-shipment-add.md) method.
2. Retrieve the basket item identifier using the [sale.basketitem.list](../basket-item/sale-basket-item-list.md) method.
3. Add the basket item to the shipment using the [sale.shipmentitem.add](./sale-shipment-item-add.md) method.
4. Check the shipment contents using the [sale.shipmentitem.list](./sale-shipment-item-list.md) method with a filter by `orderDeliveryId`.
5. Change the quantity using the [sale.shipmentitem.update](./sale-shipment-item-update.md) method or remove the product from the shipment using the [sale.shipmentitem.delete](./sale-shipment-item-delete.md) method.

## Response Format

- The add, update, and get methods return the item in `result.shipmentItem`
- The [sale.shipmentitem.delete](./sale-shipment-item-delete.md) method returns `true` in `result`
- The [sale.shipmentitem.list](./sale-shipment-item-list.md) method returns up to 50 items per call in `result.shipmentItems`, the total number of records found in `total`, and the `start` value for the next page in `next`

On error, the `error` and `error_description` fields are returned; the full list of codes is on the page of each method.

## Overview of Methods {#all-methods}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the methods: retrieve items — store manager, retrieve the field description — any user, add, update, and delete items — administrator

#| 
|| **Method** | **Description** ||
|| [sale.shipmentitem.add](./sale-shipment-item-add.md) | Adds an item to the shipment table section ||
|| [sale.shipmentitem.update](./sale-shipment-item-update.md) | Modifies an item in the shipment table section ||
|| [sale.shipmentitem.get](./sale-shipment-item-get.md) | Returns the fields of an item in the shipment table section by its identifier ||
|| [sale.shipmentitem.list](./sale-shipment-item-list.md) | Returns a list of items in the shipment table section ||
|| [sale.shipmentitem.delete](./sale-shipment-item-delete.md) | Deletes an item from the shipment table section ||
|| [sale.shipmentitem.getFields](./sale-shipment-item-get-fields.md) | Returns the available fields of an item in the shipment table section ||
|#