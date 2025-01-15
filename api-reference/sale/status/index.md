# Statuses in the Online Store: Overview of Methods

Statuses allow tracking the stages of processing and fulfilling orders. Customers can see the current order status on the website and receive notifications via e-mail.

There are two types of statuses:
- `O` — order status,
- `D` — delivery status.

> Quick navigation: [all methods](#all-methods)
> 
> User documentation: [Configure order and delivery statuses](https://helpdesk.bitrix24.com/open/17286220/)

## Default Statuses

In Bitrix24, there is a default set of statuses for orders and deliveries. Some statuses are system statuses and cannot be deleted.

#|
|| **Status Identifier** | **Type**  | **Description** | **System** ||
|| `N` | Order | Accepted, awaiting payment | Yes ||
|| `S` | Order | Shipped | No ||
|| `P` | Order | Paid, preparing for shipment | No ||
|| `D` | Order | Canceled | No ||
|| `F` | Order | Completed | Yes ||
|| `DN` | Delivery | Awaiting processing | Yes ||
|| `DD` | Delivery | Canceled | No ||
|| `DF` | Delivery | Shipped | Yes ||
|#

## Relationship of Statuses with Other Objects

**Order.** Create or modify an order using the methods [sale.order.*](../order/index.md).

**Shipments.** Create or modify a shipment using the methods [sale.shipment.*](../shipment/index.md). If the `statusId` parameter does not specify the delivery status identifier when creating a shipment, it will have the status DN "Awaiting processing."

**Localization of Statuses.** Adapt statuses for different languages using the methods [sale.statusLang.*](../status-lang/index.md).

## Overview of Methods {#all-methods}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the methods: administrator

#|
|| **Method** | **Description** ||
|| [sale.status.add](./sale-status-add.md) | Creates an order or delivery status ||
|| [sale.status.update](./sale-status-update.md) | Updates a status ||
|| [sale.status.get](./sale-status-get.md) | Returns the values of all status fields by identifier ||
|| [sale.status.list](./sale-status-list.md) | Returns a list of statuses ||
|| [sale.status.delete](./sale-status-delete.md) | Deletes a status ||
|| [sale.status.getFields](./sale-status-get-fields.md) | Returns available status fields ||
|#