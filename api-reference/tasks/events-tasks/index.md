# Overview of Events When Working with Tasks

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Events allow applications to respond to changes in almost real-time: receiving notifications about the addition, update, or deletion of a task.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to task events through:

- [outbound webhook](../../../local-integrations/local-webhooks.md)
- [application](../../../settings/app-installation/index.md) and the method [event.bind](../../events/event-bind.md)

An example of a handler code for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../events/test-handler.md).

## Data Received in Events

The handler receives the task identifier: in the [OnTaskAdd](./on-task-add.md) and [OnTaskUpdate](./on-task-update.md) events, in the `data.FIELDS_AFTER.ID` field; in the [OnTaskDelete](./on-task-delete.md) event, in the `data.FIELDS_BEFORE.ID` field.

To retrieve all task fields, use the [tasks.task.get](../tasks-task-get.md) method. After a task is deleted, the method no longer returns its data.

## Methods That Trigger OnTaskUpdate {#on-task-update-methods}

The [OnTaskUpdate](./on-task-update.md) event is triggered by the following methods:

- [tasks.task.update](../tasks-task-update.md) — updating a task
- [tasks.task.start](../status/tasks-task-start.md), [tasks.task.pause](../status/tasks-task-pause.md), [tasks.task.defer](../status/tasks-task-defer.md), [tasks.task.complete](../status/tasks-task-complete.md), [tasks.task.renew](../status/tasks-task-renew.md) — changing the task status
- [tasks.task.delegate](../tasks-task-delegate.md) — delegating a task
- [tasks.task.files.attach](../tasks-task-files-attach.md) — attaching a file to a task
- [tasks.task.startwatch](../user-actions/tasks-task-start-watch.md), [tasks.task.stopwatch](../user-actions/tasks-task-stop-watch.md) — starting and stopping watching a task
- [task.dependence.add](../task-dependence-add.md) — creating a dependency between tasks
- [task.stages.movetask](../stages/task-stages-move-task.md) — moving a task through stages

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered** ||
|| [OnTaskAdd](./on-task-add.md) | When a task is added manually or via the method [tasks.task.add](../tasks-task-add.md) ||
|| [OnTaskUpdate](./on-task-update.md) | When a task is updated manually or via REST methods. The list of methods is in the [Methods That Trigger OnTaskUpdate](#on-task-update-methods) section ||
|| [OnTaskDelete](./on-task-delete.md) | When a task is deleted manually or via the method [tasks.task.delete](../tasks-task-delete.md) ||
|#
