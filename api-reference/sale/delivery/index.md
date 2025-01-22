# Delivery Services in Online Store: Overview of Methods

In Bitrix24, two delivery options are available by default: pickup and courier delivery. Configure additional methods so that customers can choose a convenient option. To do this:

1. create a delivery service handler,
2. create a delivery service,
3. create shipping properties and link them to the delivery service,
4. add additional services if necessary.

To allow an external system to report the order status, configure transport requests.

> Quick navigation: [all methods](#all-methods)
> 
> User documentation: [Delivery Services](https://helpdesk.bitrix24.com/open/17297482/)

{% note tip "Typical use-cases and scenarios" %}

- [Set up delivery for use in CRM](../../../tutorials/sale/delivery-in-crm.md)
- [Calculate delivery costs](./webhooks/calculate.md)
- [Create a delivery order](./webhooks/create-delivery-request.md)
- [Cancel a delivery order](./webhooks/cancel-delivery-request.md)

{% endnote %}

## Linking Delivery Services with Other Objects

**Order.** Create or modify an order using the methods [sale.order.*](../order/index.md).

**Shipments.** Control the shipment of goods to customers using the methods [sale.shipment.*](../shipment/index.md).

**Shipping Properties.** If there are multiple shipments in one order, create shipping properties using the methods [sale.shipmentproperty.*](../shipment-property/index.md). For example, if there are three books in an order that need to be sent to different addresses. To specify the address for each shipment, create shipping properties.

**Property Binding.** Set conditions under which the buyer will see a specific shipping property. To do this, bind the property to the delivery service using the method [sale.propertyRelation.add](../property-relation/sale-property-relation-add.md).

## Overview of Methods {#all-methods}

### Delivery Service Handlers

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute methods: administrator

#|
|| **Method** | **Description** ||
|| [sale.delivery.handler.add](./handler/sale-delivery-handler-add.md) | Adds a delivery service handler ||
|| [sale.delivery.handler.update](./handler/sale-delivery-handler-update.md) | Modifies a delivery service handler ||
|| [sale.delivery.handler.delete](./handler/sale-delivery-handler-delete.md) | Deletes a delivery service handler ||
|| [sale.delivery.handler.list](./handler/sale-delivery-handler-list.md) | Retrieves a list of delivery service handlers ||
|#

### Delivery Services

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute methods: administrator

#|
|| **Method** | **Description** ||
|| [sale.delivery.add](./delivery/sale-delivery-add.md) | Adds a delivery service ||
|| [sale.delivery.update](./delivery/sale-delivery-update.md) | Modifies a delivery service ||
|| [sale.delivery.delete](./delivery/sale-delivery-delete.md) | Deletes a delivery service ||
|| [sale.delivery.config.update](./delivery/sale-delivery-config-update.md) | Updates delivery service settings ||
|| [sale.delivery.config.get](./delivery/sale-delivery-config-get.md) | Retrieves delivery service settings ||
|| [sale.delivery.getlist](./delivery/sale-delivery-get-list.md) | Retrieves a list of delivery services ||
|#

### Additional Services

> Scope: [`sale, delivery`](../../scopes/permissions.md)
>
> Who can execute methods: administrator

#|
|| **Method** | **Description** ||
|| [sale.delivery.extra.service.add](./extra-service/sale-delivery-extra-service-add.md) | Adds a delivery service extra service ||
|| [sale.delivery.extra.service.update](./extra-service/sale-delivery-extra-service-update.md) | Modifies a delivery service extra service ||
|| [sale.delivery.extra.service.get](./extra-service/sale-delivery-extra-service-get.md) | Returns information about all services of a specific delivery service ||
|| [sale.delivery.extra.service.delete](./extra-service/sale-delivery-extra-service-delete.md) | Deletes a delivery service extra service ||
|#

### Transport Requests

> Scope: [`sale, delivery`](../../scopes/permissions.md)
>
> Who can execute methods: administrator

#|
|| **Method** | **Description** ||
|| [sale.delivery.request.update](./delivery-request/sale-delivery-request-update.md) | Updates a transport request ||
|| [sale.delivery.request.sendmessage](./delivery-request/sale-delivery-request-send-message.md) | Creates notifications for a transport request ||
|| [sale.delivery.request.delete](./delivery-request/sale-delivery-request-delete.md) | Deletes a transport request ||
|#