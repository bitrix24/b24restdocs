# Shipment Table Section in the Online Store: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The shipment table section contains information about each product included in the shipment: name, quantity, price.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Delivery Services](https://helpdesk.bitrix24.com/open/17297482/)

## Connection of the Shipment Table Section with Other Objects

**Shipments.** Specify the shipment identifier. A list of identifiers can be obtained using the [sale.shipment.list](../shipment/sale-shipment-list.md) method.

**Cart.** Specify the cart item identifier. A list of identifiers can be obtained using the [sale.basketitem.list](../basket-item/sale-basket-item-list.md) method.

## How to Get Started

1. Create an order using [sale.order.add](../order/sale-order-add.md) or find an existing order using [sale.order.list](../order/sale-order-list.md).
2. Add cart items using [sale.basketitem.*](../basket-item/index.md).
3. Create a shipment using [sale.shipment.add](../shipment/sale-shipment-add.md).
4. Add items to the shipment using [sale.shipmentitem.add](./sale-shipment-item-add.md).
5. Check shipment items using [sale.shipmentitem.list](./sale-shipment-item-list.md).

## Overview of Methods {#all-methods}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

#| 
|| **Method** | **Description** ||
|| [sale.shipmentitem.add](./sale-shipment-item-add.md) | Adds an item to the shipment table section ||
|| [sale.shipmentitem.update](./sale-shipment-item-update.md) | Modifies an item in the shipment table section ||
|| [sale.shipmentitem.get](./sale-shipment-item-get.md) | Returns the fields of an item in the shipment table section by its identifier ||
|| [sale.shipmentitem.list](./sale-shipment-item-list.md) | Returns a list of items in the shipment table section ||
|| [sale.shipmentitem.delete](./sale-shipment-item-delete.md) | Deletes an item from the shipment table section ||
|| [sale.shipmentitem.getFields](./sale-shipment-item-get-fields.md) | Returns the available fields of an item in the shipment table section ||
|#