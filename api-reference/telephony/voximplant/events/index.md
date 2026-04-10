# Overview of Events When Handling Calls

Events allow applications to respond to changes in near real-time: receiving notifications when a call is initiated, when a conversation starts, or when it ends.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

> Quick Navigation: [All Events](#all-events)

## How to Receive Events

You can subscribe to call events through:

- [outgoing webhook](../../../../local-integrations/local-webhooks.md)

- [application](../../../../settings/app-installation/index.md) and the method [event.bind](../../../events/event-bind.md)

An example of a handler for the event is described in the article [How to Test Your Handler for Processing Events in Bitrix24](../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`telephony`](../../../scopes/permissions.md) 
>
> Who can subscribe: any user

#| 
|| **Event** | **Triggered** ||
|| [OnVoximplantCallInit](on-voximplant-call-init.md) | When a call is initiated manually or through the methods [voximplant.callback.start](../voximplant-callback-start.md), [voximplant.infocall.startwithsound](../voximplant-infocall-start-with-sound.md), [voximplant.infocall.startwithtext](../voximplant-infocall-start-with-text.md), [telephony.externalCall.register](../../telephony-external-call-register.md) ||
|| [OnVoximplantCallStart](on-voximplant-call-start.md) | When a conversation starts: when the operator answers an incoming call and when the subscriber answers an outgoing call ||
|| [OnVoximplantCallEnd](on-voximplant-call-end.md) | When a conversation ends and is recorded in history or through the method [telephony.externalCall.finish](../../telephony-external-call-finish.md) ||
|#