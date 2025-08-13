# Overview of Events When Working with Workgroups and Projects

Events allow applications to respond to changes almost in real-time: receiving notifications about the creation, updating, or deletion of workgroups and projects.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to workgroup events through:

-  [outgoing webhook](../../../local-integrations/local-webhooks.md)

-  [application](../../app-installation/index.md) and the method [event.bind](../../events/event-bind.md)

An example of a handler for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`sonet`](../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered By** ||
|| [onSonetGroupAdd](./on-sonet-group-add.md) | When a workgroup is added manually or by the method [sonet_group.create](../sonet-group-create.md) ||
|| [onSonetGroupUpdate](./on-sonet-group-update.md) | When a workgroup is edited manually or by the method [sonet_group.update](../sonet-group-update.md) ||
|| [onSonetGroupDelete](./on-sonet-group-delete.md) | When a workgroup is deleted manually or by the method [sonet_group.delete](../sonet-group-delete.md) ||
|#