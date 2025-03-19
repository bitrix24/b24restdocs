# Delivery Service Handlers in Online Stores: Overview of Methods

Delivery service handlers are templates for connecting delivery services to Bitrix24, such as CDEK or Russian Post. Handlers automatically calculate delivery costs, create orders, and track statuses.

> Quick navigation: [all methods](#all-methods)

{% note tip "Typical use-cases and scenarios" %}

- [Set up delivery for use in CRM](../../../../tutorials/sale/delivery-in-crm.md)

{% endnote %}

# URLs in the Delivery Service Handler

When creating a delivery service handler, specify the URLs:
- `CALCULATE_URL` — URL for calculating delivery costs, a required parameter. Data about the package, including products, destination address, and other parameters, will be sent to this address. In response, you will receive information about the delivery cost. See the request and response format in the article [Calculating Delivery Costs](../webhooks/calculate.md).
- `CREATE_DELIVERY_REQUEST_URL` — data about the package is sent to this address, and the delivery service processes the order. See the request and response format in the article [Creating a Delivery Order](../webhooks/create-delivery-request.md).
- `CANCEL_DELIVERY_REQUEST_URL` — use this URL if the order needs to be canceled. Data about the package is sent to this address, and the delivery service cancels the previously created order. See the request and response format in the article [Canceling a Delivery Order](../webhooks/cancel-delivery-request.md).

## Connection of Delivery Service Handlers with Other Objects

**Delivery Services.** Create a delivery service using the methods [sale.delivery.*](../delivery/index.md), specifying the symbolic code of the delivery service handler. You can obtain the symbolic codes of handlers using the method [sale.delivery.handler.list](./sale-delivery-handler-list.md).

**Transport Requests.** If you want to track changes in delivery status, set the option `HAS_CALLBACK_TRACKING_SUPPORT` to `Y`. Configure notifications using the method [sale.delivery.request.sendmessage](../delivery-request/sale-delivery-request-send-message.md).

## Overview of Methods {#all-methods}

> Scope: [`sale`](../../../scopes/permissions.md)
>
> Who can perform the methods: administrator

#|
|| **Method** | **Description** ||
|| [sale.delivery.handler.add](./sale-delivery-handler-add.md) | Adds a delivery service handler ||
|| [sale.delivery.handler.update](./sale-delivery-handler-update.md) | Modifies a delivery service handler ||
|| [sale.delivery.handler.list](./sale-delivery-handler-list.md) | Returns a list of delivery service handlers ||
|| [sale.delivery.handler.delete](./sale-delivery-handler-delete.md) | Deletes a delivery service handler ||
|#