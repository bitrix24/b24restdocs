# Overview of Events in Online Store Management

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Events allow applications to respond to changes in the online store almost in real-time: receiving notifications about the creation and deletion of orders, payments, shipments, and property values of orders.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to online store events through:

- [outgoing webhook](../../../local-integrations/local-webhooks.md)
- [application](../../../settings/app-installation/index.md) and the [event.bind](../../events/event-bind.md) method

An example of a handler code for an event is described in the article [How to Test Your Handler for Bitrix24 Event Processing](../../events/test-handler.md).

## Limitations and Features of Event Delivery

- Events are not sent to the application until the application installation is complete. More details can be found in the article [Check Application Installation](../../../settings/app-installation/installation-finish.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can subscribe: any user

#| 
|| **Event** | **Triggered** ||
|| [OnSaleOrderSaved](./on-sale-order-saved.md) | At the end of order saving ||
|| [OnSaleBeforeOrderDelete](./on-sale-before-order-delete.md) | Before order deletion ||
|| [OnPropertyValueEntitySaved](./on-property-value-entity-saved.md) | After saving order property value ||
|| [OnPaymentEntitySaved](./on-payment-entity-saved.md) | After saving payment ||
|| [OnShipmentEntitySaved](./on-shipment-entity-saved.md) | After saving shipment ||
|| [OnOrderEntitySaved](./on-order-entity-saved.md) | After saving order ||
|| [OnPropertyValueDeleted](./on-property-value-deleted.md) | When order property value is deleted ||
|| [OnPaymentDeleted](./on-payment-deleted.md) | When payment is deleted from the database ||
|| [OnShipmentDeleted](./on-shipment-deleted.md) | When shipment is deleted from the database ||
|| [OnOrderDeleted](./on-order-deleted.md) | When order is deleted from the database ||
|#