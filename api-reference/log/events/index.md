# Overview of Events When Working with the News Feed

Events allow applications to respond to changes almost in real-time: receiving notifications about the creation, updating, or deletion of messages in the News Feed.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to News Feed events through:

- [outgoing webhook](../../../local-integrations/local-webhooks.md)

- [application](../../../settings/app-installation/index.md) and the method [event.bind](../../events/event-bind.md)

An example of a handler code for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`sonet`](../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered** ||
|| [OnLiveFeedPostAdd](./on-live-feed-post-add.md) | When a message is added to the News Feed manually or via the method [log.blogpost.add](../log-blogpost-add.md) ||
|| [OnLiveFeedPostDelete](./on-live-feed-post-delete.md) | When a message is deleted from the News Feed manually or via the method [log.blogpost.delete](../log-blogpost-delete.md) ||
|| [OnLiveFeedPostUpdate](./on-live-feed-post-update.md) | When a message in the News Feed is edited manually or via the method [log.blogpost.update](../log-blogpost-update.md) ||
|#