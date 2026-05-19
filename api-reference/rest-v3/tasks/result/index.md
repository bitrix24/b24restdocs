# Task Results: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../../ai-tools/mcp.md) so the assistant can utilize the official REST documentation.

{% endnote %}

The task result captures the outcome of the work on the task. The methods in the `tasks.task.result.*` group allow you to manually add a result, create a result from a task chat message, modify the result text, delete a result, and retrieve a list of results for a task.

> Quick navigation: [all methods](#all-methods)

## REST 3.0 and Tasks

Task result methods are part of the [REST API 3.0](../../index.md). Calls are made through the address `/rest/api/`.

For general context on tasks, task chat, files, and related objects, see the section [Tasks in REST 3.0](../index.md).

## Connection to Task and Chat

**Task.** Each result is linked to a task via the `taskId` field. The task identifier can be obtained when [creating a task](../tasks-task-add.md), [retrieving a task](../tasks-task-get.md), or using the old method of [getting a list of tasks](../../../tasks/tasks-task-list.md).

**Task Chat.** The method `tasks.task.result.addfromchatmessage` creates a result from a chat message. To do this, pass the `messageId` obtained when sending a message using the [tasks.task.chat.message.send](../tasks-task-chat-message-send.md) method or the [chat message handling methods](../../../chats/messages/index.md).

## How to Get Started

1. Obtain the task identifier
2. Add a result using the [tasks.task.result.add](./tasks-task-result-add.md) method or create it from a chat message using the [tasks.task.result.addfromchatmessage](./tasks-task-result-addfromchatmessage.md) method
3. If necessary, modify the result text using the [tasks.task.result.update](./tasks-task-result-update.md) method
4. Retrieve the list of task results using the [tasks.task.result.list](./tasks-task-result-list.md) method
5. If the task result is no longer needed, delete it using the [tasks.task.result.delete](./tasks-task-result-delete.md) method

## Overview of Methods {#all-methods}

> Scope: [`tasks`](../../../scopes/permissions.md)
>
> Who can perform the method: depending on the method

#|
|| **Method** | **Description** ||
|| [tasks.task.result.add](./tasks-task-result-add.md) | Adds a result to the task ||
|| [tasks.task.result.addfromchatmessage](./tasks-task-result-addfromchatmessage.md) | Creates a result from a task chat message ||
|| [tasks.task.result.update](./tasks-task-result-update.md) | Updates the result text ||
|| [tasks.task.result.list](./tasks-task-result-list.md) | Returns a list of task results ||
|| [tasks.task.result.delete](./tasks-task-result-delete.md) | Deletes a task result ||
|#