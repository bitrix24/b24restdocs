# Overview of Events When Working with Currencies

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Events allow applications to respond to changes in almost real-time: receiving notifications about the creation, updating, or deletion of currencies in CRM.

Each event delivers only the currency identifier in the `FIELDS.ID` field. The event does not pass field values: after a currency is created or updated, retrieve them using the [crm.currency.get](../crm-currency-get.md) method from the [Currencies in CRM](../index.md) section.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to currency events through:

- [outbound webhook](../../../../local-integrations/local-webhooks.md)
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
