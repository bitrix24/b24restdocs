# Deliveries in Payments: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The methods in this section manage delivery items within CRM payments. A delivery item stores service parameters and links the payment to a specific delivery document.

This is necessary to account for logistics costs in the overall invoice. For example, you can add delivery to the payment for goods and associate it with the transport company's waybill.

> Quick navigation: [all methods](#all-methods)

## Linking Deliveries in Payments with Other Objects

**CRM Activity.** All methods in this section operate with a specific payment identified by the `paymentId`.

**Delivery Document.** The method [crm.item.payment.delivery.setDelivery](./crm-item-payment-delivery-set-delivery.md) links the delivery item to the required document via `deliveryId`.

## How to Work with Deliveries in Payment

1. Prepare the `paymentId` of the desired payment. You can obtain it using the main payment methods [crm.item.payment.*](../index.md).
2. Add a delivery item using the method [crm.item.payment.delivery.add](./crm-item-payment-delivery-add.md).
3. Check the composition of items using the method [crm.item.payment.delivery.list](./crm-item-payment-delivery-list.md).
4. If necessary, change the link to another document via [crm.item.payment.delivery.setDelivery](./crm-item-payment-delivery-set-delivery.md) or delete the item using the method [crm.item.payment.delivery.delete](./crm-item-payment-delivery-delete.md).

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the methods: depending on the method

#| 
|| **Method** | **Description** ||
|| [crm.item.payment.delivery.add](./crm-item-payment-delivery-add.md) | Adds a delivery item to the payment ||
|| [crm.item.payment.delivery.list](./crm-item-payment-delivery-list.md) | Returns a list of delivery items for a specific payment ||
|| [crm.item.payment.delivery.delete](./crm-item-payment-delivery-delete.md) | Deletes a delivery item from the payment ||
|| [crm.item.payment.delivery.setDelivery](./crm-item-payment-delivery-set-delivery.md) | Reassigns the delivery item to another delivery document ||
|#