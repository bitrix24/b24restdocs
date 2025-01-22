# Shipments in the Online Store: Overview of Methods

Shipments allow you to see which products have been sent, in what quantities, and when. This helps manage delivery statuses. An order can be shipped in parts, meaning as several independent shipments. Each shipment has its own delivery service, status, and contents.

> Quick navigation: [all methods](#all-methods)

## Connection of Shipments with Other Objects

**Order.** Create or modify an order using the methods [sale.order.*](../order/index.md).

**Delivery Services.** Create or modify a delivery service using the methods [sale.delivery.*](../delivery/delivery/index.md).

**Statuses.** Specify the delivery status identifier. Use one of the [default statuses](../status/index.md) or create a new status using the method [sale.status.add](../status/sale-status-add.md).

**Shipment Item Table.** Add or modify items in the shipment using the methods [sale.shipmentitem.*](../shipment-item/index.md).

**Shipment Properties.** If there are multiple shipments in the order, create shipment properties using the methods [sale.shipmentproperty.*](../shipment-property/index.md). For example, if there are three books in the order that need to be sent to different addresses. To specify the address for each shipment, create shipment properties.

**Linking Payments to Shipments.** Specify which shipments have been paid for. Use the methods [sale.paymentItemShipment.*](../payment-item-shipment/index.md).

## Overview of Methods {#all-methods}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can perform the methods: administrator

#| 
|| **Method** | **Description** ||
|| [sale.shipment.add](./sale-shipment-add.md) | Adds a shipment ||
|| [sale.shipment.update](./sale-shipment-update.md) | Modifies a shipment ||
|| [sale.shipment.get](./sale-shipment-get.md) | Returns values of all shipment fields ||
|| [sale.shipment.list](./sale-shipment-list.md) | Returns a list of shipments ||
|| [sale.shipment.delete](./sale-shipment-delete.md) | Deletes a shipment ||
|| [sale.shipment.getFields](./sale-shipment-get-fields.md) | Returns available shipment fields ||
|#