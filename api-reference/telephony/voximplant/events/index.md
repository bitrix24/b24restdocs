# Overview of Events When Working with Calls

Events allow applications to respond to changes in almost real-time: receiving notifications when a call is initiated, when a conversation starts, or when it ends.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to call events through:

- [outgoing webhook](../../../../local-integrations/local-webhooks.md)

- [application](../../../../settings/app-installation/index.md) and the method [event.bind](../../../events/event-bind.md)

An example of a handler for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`telephony`](../../../scopes/permissions.md) 
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered by** ||
|| [OnVoximplantCallInit](on-voximplant-call-init.md) | When a call is manually initialized or through the methods [voximplant.callback.start](../voximplant-callback-start.md), [voximplant.infocall.startwithsound](../voximplant-infocall-start-with-sound.md), [voximplant.infocall.startwithtext](../voximplant-infocall-start-with-text.md), [telephony.externalcall.register](../../telephony-external-call-register.md) ||
|| [OnVoximplantCallStart](on-voximplant-call-start.md) | When a conversation starts: when the operator answers an incoming call and when the subscriber answers an outgoing call ||
|| [OnVoximplantCallEnd](on-voximplant-call-end.md) | When a conversation ends and is recorded in history or through the method [telephony.externalcall.finish](../../telephony-external-call-finish.md) ||
|#