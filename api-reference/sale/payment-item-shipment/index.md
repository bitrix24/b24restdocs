# Linking Payments to Shipments in the Online Store: Overview of Methods

An order can include multiple products that need to be shipped in parts, meaning as several independent shipments. To allocate payments among shipments, use the linking of payments to shipments.

> Quick navigation: [all methods](#all-methods)

## Connection of Payment Linking to Shipments with Other Objects

**Shipments.** Specify the ID of the paid shipment. You can obtain a list of shipment IDs using the method [sale.shipment.list](../shipment/sale-shipment-list.md).

**Payments.** Specify the ID of the payment that needs to be linked to the shipment. You can obtain a list of payment IDs using the method [sale.payment.list](../payment/sale-payment-list.md).

## Overview of Methods {#all-methods}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the methods: administrator

#|
|| **Method** | **Description** ||
|| [sale.paymentItemShipment.add](./sale-payment-item-shipment-add.md) | Adds a payment link to a shipment ||
|| [sale.paymentItemShipment.update](./sale-payment-item-shipment-update.md) | Modifies the payment link to a shipment ||
|| [sale.paymentItemShipment.get](./sale-payment-item-shipment-get.md) | Returns the values of the payment link fields for a shipment by its ID ||
|| [sale.paymentItemShipment.list](./sale-payment-item-shipment-list.md) | Returns a list of payment links to shipments based on a filter ||
|| [sale.paymentItemShipment.delete](./sale-payment-item-shipment-delete.md) | Deletes the payment link to a shipment ||
|| [sale.paymentItemShipment.getFields](./sale-payment-item-shipment-get-fields.md) | Returns the fields of the payment link to a shipment ||
|#