# Overview of Events When Working with Estimates

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Events allow applications to respond to changes in almost real-time: receiving notifications about the creation, updating, or deletion of estimates. The methods for working with estimates are collected in the section [Estimates](../index.md).

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to estimate events through:

- [outbound webhook](../../../../local-integrations/local-webhooks.md)
- [application](../../../../settings/app-installation/index.md) and the method [event.bind](../../../events/event-bind.md)

An example of a handler code for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../events/test-handler.md).

## What the Handler Receives

Estimate events do not pass the entire object — the handler receives only the identifier by which the estimate can be requested with a method.

The `data.FIELDS.ID` field contains the estimate identifier. The rest of the data is returned by the method [crm.quote.get](../crm-quote-get.md) or by the universal method [crm.item.get](../../universal/crm-item-get.md) with `entityTypeId = 7`. After an estimate is deleted, its data can no longer be retrieved by identifier, so only the identifier itself is available in the `onCrmQuoteDelete` handler.

The `onCrmQuoteUpdate` event does not report which fields were changed. To track a change to a specific field, retain the previous state of the estimate on your side and compare it with the response of [crm.quote.get](../crm-quote-get.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered** ||
|| [onCrmQuoteAdd](./on-crm-quote-add.md) | When an estimate is created manually or via the method [crm.quote.add](../crm-quote-add.md) ||
|| [onCrmQuoteUpdate](./on-crm-quote-update.md) | When an estimate is updated manually or via the method [crm.quote.update](../crm-quote-update.md) ||
|| [onCrmQuoteDelete](./on-crm-quote-delete.md) | When an estimate is deleted manually or via the method [crm.quote.delete](../crm-quote-delete.md) ||
|#
