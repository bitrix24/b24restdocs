# Overview of Events in Telephony

Events allow applications to respond to changes almost in real-time: receiving notifications when a call is initiated through the CRM object or upon a request for a callback from the CRM widget.

Detailed work with events is described in the article [Concept and Benefits of Event Processing](../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to the event [OnExternalCallBackStart](on-external-call-back-start.md) via:

- [outgoing webhook](../../../local-integrations/local-webhooks.md)

- [application](../../app-installation/index.md) and the method [event.bind](../../events/event-bind.md)

You can subscribe to the event [OnExternalCallStart](on-external-call-start.md) only through the [application](../../app-installation/index.md) and the method [event.bind](../../events/event-bind.md).

An example of a handler code for the event is described in the article [How to Test Your Handler for Bitrix24 Event Processing](../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`telephony`](../../scopes/permissions.md) 
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered by** ||
|| [OnExternalCallStart](on-external-call-start.md) | When clicking on a phone number in CRM entities to make an outgoing call ||
|| [OnExternalCallBackStart](on-external-call-back-start.md) | When filling out the CRM callback form ||
|#