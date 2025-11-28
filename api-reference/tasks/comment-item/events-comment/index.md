# Overview of Events When Working with Task Comments

Events allow applications to respond to changes almost in real-time: receiving notifications about the addition, update, or deletion of a comment in a task.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to task events through:

-  [outgoing webhook](../../../../local-integrations/local-webhooks.md)
-  [application](../../../../settings/app-installation/index.md) and the method [event.bind](../../../events/event-bind.md)

An example of a handler for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered By** ||
|| [OnTaskCommentAdd](./on-task-comment-add.md) | When a comment is added to a task manually or via the method [task.commentitem.add](../task-comment-item-add.md) for the old detail form. When a message is added to the task chat for the new detail form ||
|| [OnTaskCommentUpdate](./on-task-comment-update.md) | When a comment in a task is updated manually or via the method [task.commentitem.update](../task-comment-item-update.md) for the old detail form. When a message in the task chat is changed for the new detail form ||
|| [OnTaskCommentDelete](./on-task-comment-delete.md) | When a comment in a task is deleted manually or via the method [task.commentitem.delete](../task-comment-item-delete.md) for the old detail form. When a message is deleted from the task chat for the new detail form ||
|#