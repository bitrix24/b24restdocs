# Overview of Events When Working with Currencies

Events allow applications to respond to changes in almost real-time: receiving notifications about the creation, updating, or deletion of currencies in CRM.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to currency events through:

- [outgoing webhook](../../../../local-integrations/local-webhooks.md)

- [application](../../../../settings/app-installation/index.md) and the method [event.bind](../../../events/event-bind.md)

An example of a handler for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered** ||
|| [onCrmCurrencyAdd](./on-crm-currency-add.md) | When a currency is added manually or via the method [crm.currency.add](../crm-currency-add.md) ||
|| [onCrmCurrencyUpdate](./on-crm-currency-update.md) | When a currency is changed manually or via the method [crm.currency.update](../crm-currency-update.md) ||
|| [onCrmCurrencyDelete](./on-crm-currency-delete.md) | When a currency is deleted manually or via the method [crm.currency.delete](../crm-currency-delete.md) ||
|#