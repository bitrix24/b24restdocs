# Deliveries in Universal CRM Objects: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The methods in this section work with deliveries associated with universal CRM objects. For example, you can retrieve a list of deliveries for a deal or get details about a specific shipment by lead.

This is necessary to integrate logistics services with CRM data. Bitrix24 stores delivery information separately from the main fields of the object but links them through identifiers.

> Quick navigation: [all methods](#all-methods)

## Linking Deliveries to CRM Objects

**CRM Object.** The connection is made through the pair of parameters `entityTypeId` and `entityId`. The first indicates the [type of CRM object](../../data-types.md#object_type), while the second refers to a specific element of that type.

**Delivery Service.** Each delivery record contains a reference to a configured delivery service. The delivery service identifier can be obtained using the [sale.delivery.getlist](../../../sale/delivery/delivery/sale-delivery-get-list.md) method. In the current section, `deliveryId` is returned by the [crm.item.delivery.list](./crm-item-delivery-list.md) and [crm.item.delivery.get](./crm-item-delivery-get.md) methods.

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