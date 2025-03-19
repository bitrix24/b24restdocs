# Delivery Services in Online Stores: Overview of Methods

Delivery services automate the calculation of shipping costs, order creation, and tracking their statuses. To connect a new delivery service, first create a [handler](../handler/index.md) for the delivery service.

> Quick navigation: [all methods](#all-methods)
> 
> User documentation: [Delivery Services](https://helpdesk.bitrix24.com/open/17297482/)

{% note tip "Typical use-cases and scenarios" %}

- [Set up delivery for use in CRM](../../../../tutorials/sale/delivery-in-crm.md)

{% endnote %}

## Connection of Delivery Services with Other Objects

**Delivery Service Handlers.** Specify the symbolic code of the delivery service handler. You can obtain the symbolic codes of handlers using the [sale.delivery.handler.list](../handler/sale-delivery-handler-list.md) method.

**Additional Services.** Add additional services for the delivery service, such as "Doorstep Delivery." Use the [sale.delivery.extra.service.*](../extra-service/index.md) methods.

**Transport Requests.** To track changes in delivery status, set up notifications from the external transport service using the [sale.delivery.request.sendmessage](../delivery-request/sale-delivery-request-send-message.md) method.

**Order.** Create or modify an order using the [sale.order.*](../../order/index.md) methods.

**Shipments.** Control the shipment of goods to customers using the [sale.shipment.*](../../shipment/index.md) methods.

**Shipment Properties.** If there are multiple shipments in one order, create shipment properties using the [sale.shipmentproperty.*](../../shipment-property/index.md) methods. For example, if there are three books in an order that need to be sent to different addresses, create shipment properties to specify the address for each shipment.

**Property Binding.** Set conditions under which the buyer will see a specific shipment property. To ensure the buyer sees the "Floor" property only when selecting courier delivery, bind the property to the delivery service using the [sale.propertyRelation.add](../../property-relation/sale-property-relation-add.md) method.

## Overview of Methods {#all-methods}

> Scope: [`sale`](../../../scopes/permissions.md)
>
> Who can execute methods: administrator

#|
|| **Method** | **Description** ||
|| [sale.delivery.add](./sale-delivery-add.md) | Adds a delivery service ||
|| [sale.delivery.update](./sale-delivery-update.md) | Updates a delivery service ||
|| [sale.delivery.config.update](./sale-delivery-config-update.md) | Updates delivery service settings ||
|| [sale.delivery.config.get](./sale-delivery-config-get.md) | Returns delivery service settings ||
|| [sale.delivery.getlist](./sale-delivery-get-list.md) | Returns a list of delivery services ||
|| [sale.delivery.delete](./sale-delivery-delete.md) | Deletes a delivery service ||
|#