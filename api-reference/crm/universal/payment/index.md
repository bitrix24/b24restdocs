# Payments in Universal CRM Objects: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The methods in this section work with payments associated with universal CRM objects. For example, you can create a payment for a deal, mark the receipt of funds, or generate a payment link for the client.

This is necessary to synchronize the status and parameters of payments in the CRM with external payment systems. Bitrix24 stores payment data separately from the main fields of the object but links them through identifiers.

> Quick navigation: [all methods](#all-methods)

## Linking Payments to Other Objects

**CRM Object.** To create and retrieve a list of payments, use the parameter pair `entityTypeId` and `entityId`. The first indicates the [type of CRM object](../../data-types.md#object_type), while the second refers to a specific element of that type.

**Payment Systems.** The payment data returns `paySystemId` and `paySystemName`. You can access available payment systems using the [sale.paysystem.list](../../../pay-system/sale-pay-system-list.md) method.

**Payment Line Items.** Each payment can contain a set of line items. To manage the composition of the payment, use the methods in the section [Line Items in Payment](./products-in-payment/index.md).

**Delivery Items.** A payment can be linked to delivery items. To work with them, use the methods in the section [Deliveries in Payments](./delivery-in-payment/index.md).

## How to Work with Payments in CRM

1. Define the CRM object type `entityTypeId` and its identifier `entityId`.
2. Create a payment using the [crm.item.payment.add](./crm-item-payment-add.md) method.
3. If necessary, add products to the payment using the methods in the section [Line Items in Payment](./products-in-payment/index.md) or delivery services using the methods in the section [Deliveries in Payments](./delivery-in-payment/index.md).
5. Retrieve the list of payments for the object using the [crm.item.payment.list](./crm-item-payment-list.md) method and select the desired payment by `id`.
6. Get the details of a specific payment using the [crm.item.payment.get](./crm-item-payment-get.md) method and update the data if necessary using the [crm.item.payment.update](./crm-item-payment-update.md) method.
7. Change the payment status using the [crm.item.payment.pay](./crm-item-payment-pay.md) or [crm.item.payment.unpay](./crm-item-payment-unpay.md) methods.
8. If a payment link for the client is needed, use [salescenter.payment.getPublicUrl](./salescenter-payment-get-public-url.md).
9. If the payment is no longer needed, delete it using the [crm.item.payment.delete](./crm-item-payment-delete.md) method.

## Limitations and Features

- For the `crm.item.payment.*` methods, a correct pair of `entityTypeId` and `entityId` for the CRM object is required.
- The methods [crm.item.payment.product.*](./products-in-payment/index.md) and [crm.item.payment.delivery.*](./delivery-in-payment/index.md) only work with an existing payment by `paymentId`.
- In list methods, apply `filter` and `order` in a format compatible with [sale.payment.list](../../../sale/payment/sale-payment-list.md).
- The [salescenter.payment.getPublicUrl](./salescenter-payment-get-public-url.md) method works in the `salescenter` scope and is used only for generating a public link.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md), [`salescenter`](../../../scopes/permissions.md)
>
> Who can execute the methods: depending on the method

### Main Payment Methods

#|
|| **Method** | **Description** ||
|| [crm.item.payment.add](./crm-item-payment-add.md) | Creates a payment for a CRM object ||
|| [crm.item.payment.update](./crm-item-payment-update.md) | Modifies the set of payment fields ||
|| [crm.item.payment.get](./crm-item-payment-get.md) | Returns brief information about the payment ||
|| [crm.item.payment.list](./crm-item-payment-list.md) | Returns a list of payments for a specific CRM object ||
|| [crm.item.payment.delete](./crm-item-payment-delete.md) | Deletes a payment ||
|| [crm.item.payment.pay](./crm-item-payment-pay.md) | Changes the payment status to "Paid" ||
|| [crm.item.payment.unpay](./crm-item-payment-unpay.md) | Changes the payment status to "Unpaid" ||
|#

### Public Payment Link

#|
|| **Method** | **Description** ||
|| [salescenter.payment.getPublicUrl](./salescenter-payment-get-public-url.md) | Generates a public payment link ||
|#

### Line Items in Payment

#|
|| **Method** | **Description** ||
|| [crm.item.payment.product.add](./products-in-payment/crm-item-payment-product-add.md) | Adds a line item to the payment ||
|| [crm.item.payment.product.list](./products-in-payment/crm-item-payment-product-list.md) | Returns a list of line items in the payment ||
|| [crm.item.payment.product.delete](./products-in-payment/crm-item-payment-product-delete.md) | Deletes a line item from the payment ||
|| [crm.item.payment.product.setQuantity](./products-in-payment/crm-item-payment-product-set-quantity.md) | Changes the quantity of a product in the payment line item ||
|#

### Deliveries in Payments

#|
|| **Method** | **Description** ||
|| [crm.item.payment.delivery.add](./delivery-in-payment/crm-item-payment-delivery-add.md) | Adds a delivery item to the payment ||
|| [crm.item.payment.delivery.list](./delivery-in-payment/crm-item-payment-delivery-list.md) | Returns a list of delivery items for a specific payment ||
|| [crm.item.payment.delivery.delete](./delivery-in-payment/crm-item-payment-delivery-delete.md) | Deletes a delivery item from the payment ||
|| [crm.item.payment.delivery.setDelivery](./delivery-in-payment/crm-item-payment-delivery-set-delivery.md) | Reassigns a delivery item to another delivery document ||
|#