# Shipment Table Section in the Online Store: Overview of Methods

The shipment table section contains information about each product included in the shipment: name, quantity, price.

> Quick navigation: [all methods](#all-methods)

## Connection of the Shipment Table Section with Other Objects

**Shipments.** Specify the shipment identifier. A list of identifiers can be obtained using the [sale.shipment.list](../shipment/sale-shipment-list.md) method.

**Cart.** Specify the cart item identifier. A list of identifiers can be obtained using the [sale.basketitem.list](../basket-item/sale-basket-item-list.md) method.

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