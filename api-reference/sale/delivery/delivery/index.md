# Delivery Services in Online Stores: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Delivery services automate the calculation of shipping costs, order creation, and tracking their statuses. To connect a new delivery service, first create a [handler](../handler/index.md) for the delivery service.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Delivery Services](https://helpdesk.bitrix24.com/open/17297482/)

{% note tip "Typical use-cases and scenarios" %}

- [Set up delivery for use in CRM](../../../../tutorials/sale/delivery-in-crm.md)

{% endnote %}

## Connection of Delivery Services with Other Objects

**Delivery Service Handlers.** A delivery service is created from a REST handler: pass its symbolic code in the `REST_CODE` parameter of the [sale.delivery.add](./sale-delivery-add.md) method. You can obtain the symbolic codes of handlers using the [sale.delivery.handler.list](../handler/sale-delivery-handler-list.md) method. The handler defines the URLs for calculating and ordering delivery, the profiles, and the list of service settings.

**Delivery Profiles.** When a service is created, a child delivery service is created automatically for each handler profile. Profile identifiers are returned in the `profiles` key of the [sale.delivery.add](./sale-delivery-add.md) response, and the `PARENT_ID` field of each profile contains the identifier of the parent service. To find the profiles of a service, use the [sale.delivery.getlist](./sale-delivery-get-list.md) method with a filter by `PARENT_ID`.

**Service Settings.** Values of the settings declared in the handler's `SETTINGS.CONFIG` are set with the [sale.delivery.config.update](./sale-delivery-config-update.md) method and retrieved with the [sale.delivery.config.get](./sale-delivery-config-get.md) method.

**Additional Services.** Add additional services for the delivery service, such as "Doorstep Delivery." Use the [sale.delivery.extra.service.*](../extra-service/index.md) methods.

**Transport Requests.** To track changes in delivery status, set up notifications from the external transport service using the [sale.delivery.request.sendmessage](../delivery-request/sale-delivery-request-send-message.md) method.

**Order.** Create or modify an order using the [sale.order.*](../../order/index.md) methods.

**Shipments.** Control the shipment of goods to customers using the [sale.shipment.*](../../shipment/index.md) methods.

**Shipment Properties.** If there are multiple shipments in one order, create shipment properties using the [sale.shipmentproperty.*](../../shipment-property/index.md) methods. For example, if there are three books in an order that need to be sent to different addresses, create shipment properties to specify the address for each shipment.

**Property Binding.** Set conditions under which the buyer will see a specific shipment property. To ensure the buyer sees the "Floor" property only when selecting courier delivery, bind the property to the delivery service using the [sale.propertyRelation.add](../../property-relation/sale-property-relation-add.md) method.

## How to Get Started

1. Create a delivery service handler using [sale.delivery.handler.add](../handler/sale-delivery-handler-add.md) or retrieve the code of an existing one using [sale.delivery.handler.list](../handler/sale-delivery-handler-list.md).
2. Create a delivery service using [sale.delivery.add](./sale-delivery-add.md), passing the handler code in `REST_CODE`. Save the service and profile identifiers from the response.
3. Set the service settings values using [sale.delivery.config.update](./sale-delivery-config-update.md) and check them using [sale.delivery.config.get](./sale-delivery-config-get.md).
4. Change the name, currency, activity, or logo using [sale.delivery.update](./sale-delivery-update.md).
5. Check the list of services using [sale.delivery.getlist](./sale-delivery-get-list.md).
6. If the service is no longer used, delete it using [sale.delivery.delete](./sale-delivery-delete.md). The service profiles are deleted along with the service.

## Limitations and Specifics

- The `update`, `delete`, `config.update`, and `config.get` methods work only with services and profiles created from REST handlers. For other services, such as "Local pickup", the methods return the `ACCESS_DENIED` error. The `getlist` method returns all delivery services
- An application can modify only services created from its own handlers
- Pass parameter names in uppercase: `NAME`, `CURRENCY`, `SELECT`. The method does not recognize parameters in lowercase
- The `config.update` method replaces the entire set of settings: settings not passed in `CONFIG` are deleted

## Overview of Methods {#all-methods}

> Scope: [`delivery`](../../../scopes/permissions.md)
>
> Who can execute methods: CRM administrator

#|
|| **Method** | **Description** ||
|| [sale.delivery.add](./sale-delivery-add.md) | Adds a delivery service ||
|| [sale.delivery.update](./sale-delivery-update.md) | Updates a delivery service ||
|| [sale.delivery.getlist](./sale-delivery-get-list.md) | Returns a list of delivery services ||
|| [sale.delivery.delete](./sale-delivery-delete.md) | Deletes a delivery service ||
|| [sale.delivery.config.update](./sale-delivery-config-update.md) | Updates delivery service settings ||
|| [sale.delivery.config.get](./sale-delivery-config-get.md) | Returns delivery service settings ||
|#