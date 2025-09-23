# Overview of Events When Working with Tasks

Events allow applications to respond to changes in almost real-time: receiving notifications about the addition, update, or deletion of a task.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to task events through:

-  [outgoing webhook](../../../local-integrations/local-webhooks.md)
-  [application](../../../settings/app-installation/index.md) and the method [event.bind](../../events/event-bind.md)

An example of a handler code for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered** ||
|| [OnTaskAdd](./on-task-add.md) | When a task is added manually or via the method [task.task.add](../tasks-task-add.md) ||
|| [OnTaskUpdate](./on-task-update.md) | When a task is updated manually or through methods:
- [task update](../tasks-task-update.md)
- status update, for example, via the method [setting status to Completed](../tasks-task-complete.md)
- [delegation](../tasks-task-delegate.md)
- [attaching a file to a task](../tasks-task-files-attach.md)
- observing a task, for example, via the method [starting observation](../tasks-task-start-watch.md)
- managing dependencies between tasks, for example, via the method [creating a dependency](../task-dependence-add.md)
- [moving a task through stages](../stages/task-stages-move-task.md) ||
|| [OnTaskDelete](./on-task-delete.md) | When a task is deleted manually or via the method [task.task.delete](../tasks-task-delete.md) ||
|#