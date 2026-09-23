# Deliveries in Universal CRM Objects: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The methods `crm.item.delivery.*` work with the deliveries of universal CRM objects. For example, you can retrieve a list of deliveries for a deal or brief information about a single delivery of an invoice.

A delivery is a shipment of an order. Bitrix24 stores it separately from the fields of the CRM object: a delivery belongs to an order, and the order is linked to a deal or an invoice.

{% note info "" %}

The methods `crm.item.delivery.*` are read-only and return a brief set of delivery fields. To create a shipment or retrieve all of its fields, use the Bitrix24 interface or the [sale.shipment.*](../../../sale/shipment/index.md) methods, which are available to administrators only.

{% endnote %}

> Quick navigation: [all methods](#all-methods)

## Linking Deliveries to Other Objects

**CRM Object.** The [crm.item.delivery.list](./crm-item-delivery-list.md) method finds deliveries by the pair of parameters `entityTypeId` and `entityId`. The first indicates the [type of CRM object](../../data-types.md#object_type), while the second refers to a specific element of that type.

#|
|| **CRM Object** | **`entityTypeId`** ||
|| Deal | `2` ||
|| Invoice | `31` ||
|#

Other objects, such as a lead or a smart process, never have deliveries — for them, the `crm.item.delivery.list` method returns an empty list.

**Order.** An online store order is linked to a CRM object by a separate binding record. You can check and create such a binding using the [crm.orderentity.*](../order-entity/index.md) methods.

**Delivery Service.** Each delivery contains the delivery service identifier `deliveryId`. The list of delivery services with their identifiers is returned by the [sale.delivery.getlist](../../../sale/delivery/delivery/sale-delivery-get-list.md) method.

**Payment.** A single delivery can be partially or fully paid. Delivery items within a payment are handled by the [crm.item.payment.delivery.*](../payment/delivery-in-payment/index.md) methods, and the payments themselves by the [crm.item.payment.*](../payment/index.md) methods.

## How to Work with Deliveries in CRM

1. Determine the CRM object type `entityTypeId` and the identifier of the object itself `entityId`.
2. Retrieve the list of the object's deliveries using the [crm.item.delivery.list](./crm-item-delivery-list.md) method.
3. Select the desired delivery from the list and obtain information using its `id` with the [crm.item.delivery.get](./crm-item-delivery-get.md) method.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can perform the methods: depending on the method

#| 
|| **Method** | **Description** ||
|| [crm.item.delivery.get](./crm-item-delivery-get.md) | Returns brief information about the delivery by identifier ||
|| [crm.item.delivery.list](./crm-item-delivery-list.md) | Returns a list of deliveries for the CRM object ||
|#

## Continue Learning

- [{#T}](../payment/index.md)
- [{#T}](../payment/delivery-in-payment/index.md)
- [{#T}](../index.md)