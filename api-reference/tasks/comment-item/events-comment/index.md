# Overview of Events When Working with Task Comments

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Events allow applications to respond to changes almost in real-time: receiving notifications about the addition, update, or deletion of a comment in a task.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to task comment events through:

- [outbound webhook](../../../../local-integrations/local-webhooks.md)
- [application](../../../../settings/app-installation/index.md) and the method [event.bind](../../../events/event-bind.md)

An example of a handler for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../events/test-handler.md).

## Which Events Are Sent in the Old and New Task Card

Which events are sent depends on the task card:
- old task card, before module version `tasks 25.700.0` — all three events: comment added, updated, and deleted
- new task card, starting from module version `tasks 25.700.0` — only [OnTaskCommentAdd](./on-task-comment-add.md)

In the new task card, comments became task chat messages. The event is sent when a message is added to the chat.

The structure of the new task card is described in the article [New Task Card: Overview of Changes](../../tasks-new.md).

## What Data the Events Contain

The events do not contain the comment text, only identifiers in `data.FIELDS_AFTER`. The update and delete events also contain the `ACTION` field. How to retrieve a comment by its identifiers depends on the task card.

In the old task card, the comment `ID` and `TASK_ID` are sent. To retrieve the comment, pass them to the [task.commentitem.get](../task-comment-item-get.md) method: `TASK_ID` in `TASKID`, `ID` in `ITEMID`. The method does not return a deleted comment.

In the new task card, `ID` with the value `0`, `MESSAGE_ID`, and `TASK_ID` are sent. To retrieve the message:
1. call the [tasks.task.get](../../tasks-task-get.md) method with `taskId` equal to `TASK_ID` — the task chat ID is returned in `result.task.chatId`
2. call the [im.dialog.messages.get](../../../chats/messages/im-dialog-messages-get.md) method with `DIALOG_ID` equal to `chat{chatId}` — it returns the latest chat messages
3. find the message in `result.messages` whose `id` equals `MESSAGE_ID`. If it is not among the latest messages, load earlier ones: pass the minimum `id` of the received messages in `LAST_ID`

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered By** ||
|| [OnTaskCommentAdd](./on-task-comment-add.md) | When a comment is added to a task manually or via the method [task.commentitem.add](../task-comment-item-add.md) in the old task card. When a message is added to the task chat, including via the method [tasks.task.chat.message.send](../../tasks-task-chat-message-send.md), in the new task card ||
|| [OnTaskCommentUpdate](./on-task-comment-update.md) | When updating a comment in a task manually or using the [task.commentitem.update](../task-comment-item-update.md) method. Only in the old task card ||
|| [OnTaskCommentDelete](./on-task-comment-delete.md) | When deleting a comment in a task manually or using the [task.commentitem.delete](../task-comment-item-delete.md) method. Only in the old task card ||
|#
