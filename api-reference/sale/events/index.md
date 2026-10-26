# Overview of Events in Online Store Management

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Events allow applications to respond to changes in the online store almost in real-time: receiving notifications about the creation and deletion of orders, payments, shipments, and property values of orders.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../events/index.md).

> Quick navigation: [all events](#all-events)
>
> User documentation: [Place an Order on the Website](https://helpdesk.bitrix24.com/open/17300484/)

## How to Receive Events

You can subscribe to online store events through:

- [outbound webhook](../../../local-integrations/local-webhooks.md)
- [application](../../../settings/app-installation/index.md) and the [event.bind](../../events/event-bind.md) method

An example of a handler code for an event is described in the article [How to Test Your Handler for Bitrix24 Event Processing](../../events/test-handler.md).

## Limitations and Features of Event Delivery

- Events are not sent to the application until the application installation is complete. More details can be found in the article [Check Application Installation](../../../settings/app-installation/installation-finish.md).
- The handler receives the identifier of the modified object. Request the complete information about the object using a dedicated method: [sale.order.get](../order/sale-order-get.md), [sale.payment.get](../payment/sale-payment-get.md), [sale.shipment.get](../shipment/sale-shipment-get.md), or the methods of the [Property Values](../property-value/index.md) section.
- Events are delivered after the request that triggered them completes. For deletion events, the object has already been deleted by then, so the dedicated method will not return its data. If the handler needs the fields of the object being deleted, store them in advance using the save events.

## Which Event to Choose

All events pass the object identifier `ID` in `data.FIELDS`. The [OnSaleOrderSaved](./on-sale-order-saved.md) and [OnSaleBeforeOrderDelete](./on-sale-before-order-delete.md) events also pass the external order identifier `XML_ID` and the action `ACTION` with the value `save` or `delete`.

- To retrieve an order after any change, subscribe to [OnSaleOrderSaved](./on-sale-order-saved.md). The event arrives at the end of saving, when the order, payments, shipments, and property values have already been saved. Within a single request to Bitrix24, the handler receives it once, even if the order was saved several times.
- To track every save of the order fields, use [OnOrderEntitySaved](./on-order-entity-saved.md). The event arrives right after the order fields are written, before payments, shipments, and property values are saved.
- To retrieve the external identifier `XML_ID` of an order being deleted, use [OnSaleBeforeOrderDelete](./on-sale-before-order-delete.md). To find out that an order has been deleted from the database, use [OnOrderDeleted](./on-order-deleted.md).

### Order of Events When an Order Is Deleted

When an order is deleted manually or with the [sale.order.delete](../order/sale-order-delete.md) method, events arrive in the following order:

1. The order is canceled and saved: [OnOrderEntitySaved](./on-order-entity-saved.md) and [OnSaleOrderSaved](./on-sale-order-saved.md) arrive.
2. [OnSaleBeforeOrderDelete](./on-sale-before-order-delete.md). After it, `OnSaleOrderSaved` is not sent for this order.
3. [OnPropertyValueDeleted](./on-property-value-deleted.md), [OnPaymentDeleted](./on-payment-deleted.md), and [OnShipmentDeleted](./on-shipment-deleted.md) for each property value, payment, and shipment of the order.
4. [OnOrderDeleted](./on-order-deleted.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can subscribe: any user

#| 
|| **Event** | **Triggered** ||
|| [OnSaleOrderSaved](./on-sale-order-saved.md) | At the end of saving an order manually or with the [sale.order.add](../order/sale-order-add.md), [sale.order.update](../order/sale-order-update.md) methods, as well as with the methods for payments, shipments, and order property values ||
|| [OnSaleBeforeOrderDelete](./on-sale-before-order-delete.md) | Before an order is deleted from the database manually or with the [sale.order.delete](../order/sale-order-delete.md) method ||
|| [OnPropertyValueEntitySaved](./on-property-value-entity-saved.md) | After an order property value is saved manually or with the [sale.propertyvalue.modify](../property-value/sale-property-value-modify.md) method ||
|| [OnPaymentEntitySaved](./on-payment-entity-saved.md) | After a payment is saved manually or with the [sale.payment.add](../payment/sale-payment-add.md), [sale.payment.update](../payment/sale-payment-update.md) methods ||
|| [OnShipmentEntitySaved](./on-shipment-entity-saved.md) | After a shipment is saved manually or with the [sale.shipment.add](../shipment/sale-shipment-add.md), [sale.shipment.update](../shipment/sale-shipment-update.md) methods ||
|| [OnOrderEntitySaved](./on-order-entity-saved.md) | Each time an order is saved manually or with the [sale.order.add](../order/sale-order-add.md), [sale.order.update](../order/sale-order-update.md) methods or the methods for payments, shipments, and order property values, and also when an order is deleted ||
|| [OnPropertyValueDeleted](./on-property-value-deleted.md) | When an order property value is deleted from the database manually, with the [sale.propertyvalue.delete](../property-value/sale-property-value-delete.md) method, or together with the order ||
|| [OnPaymentDeleted](./on-payment-deleted.md) | When a payment is deleted from the database manually, with the [sale.payment.delete](../payment/sale-payment-delete.md) method, or together with the order ||
|| [OnShipmentDeleted](./on-shipment-deleted.md) | When a shipment is deleted from the database manually, with the [sale.shipment.delete](../shipment/sale-shipment-delete.md) method, or together with the order ||
|| [OnOrderDeleted](./on-order-deleted.md) | After an order is deleted from the database manually or with the [sale.order.delete](../order/sale-order-delete.md) method ||
|#