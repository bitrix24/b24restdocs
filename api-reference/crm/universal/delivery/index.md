# Deliveries in Universal CRM Objects: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The methods `crm.item.delivery.*` work with deliveries associated with universal CRM objects. For example, you can retrieve a list of deliveries for a deal or get details about a specific shipment by invoice.

This is necessary to integrate logistics services with CRM data. Bitrix24 stores delivery information separately from the main fields of the object but links them through identifiers.

{% note info "" %}

The methods `crm.item.delivery.*` are read-only: they retrieve a delivery and a list of deliveries, but do not create, modify, or delete them. Deliveries themselves are created in the Bitrix24 interface while working with a CRM object card. A delivery can be linked to a payment using the [crm.item.payment.delivery.*](../payment/delivery-in-payment/index.md) methods.

{% endnote %}

> Quick navigation: [all methods](#all-methods)

## Linking Deliveries to CRM Objects

**CRM Object.** The connection is made through the pair of parameters `entityTypeId` and `entityId`. The first indicates the [type of CRM object](../../data-types.md#object_type), while the second refers to a specific element of that type. Deliveries appear for objects that have an order attached — deals with `entityTypeId = 2` and invoices with `entityTypeId = 31`. If the object has no orders, the [crm.item.delivery.list](./crm-item-delivery-list.md) method returns an empty list.

**Delivery Service.** Each delivery record contains a reference to a configured delivery service. The delivery service identifier can be obtained using the [sale.delivery.getlist](../../../sale/delivery/delivery/sale-delivery-get-list.md) method. In the current section, `deliveryId` is returned by the [crm.item.delivery.list](./crm-item-delivery-list.md) and [crm.item.delivery.get](./crm-item-delivery-get.md) methods.

**Payments.** A single delivery can be partially or fully paid. Delivery items within a payment are handled by the [crm.item.payment.delivery.*](../payment/delivery-in-payment/index.md) methods, and the payments themselves by the [crm.item.payment.*](../payment/index.md) methods.

## How to Work with Deliveries in CRM

1. Determine the CRM object type `entityTypeId` and its identifier `entityId`.
2. Retrieve the list of deliveries associated with the object using the [crm.item.delivery.list](./crm-item-delivery-list.md) method.
3. Select the desired delivery from the list and obtain information using its `id` with the [crm.item.delivery.get](./crm-item-delivery-get.md) method.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can perform the methods: depending on the method

#| 
|| **Method** | **Description** ||
|| [crm.item.delivery.get](./crm-item-delivery-get.md) | Returns information about the delivery by identifier ||
|| [crm.item.delivery.list](./crm-item-delivery-list.md) | Returns a list of deliveries for the CRM object ||
|#

## Continue Learning

- [{#T}](../payment/index.md)
- [{#T}](../payment/delivery-in-payment/index.md)
- [{#T}](../index.md)