# New Task Card: Overview of Changes

The new task card has moved comments to chat. The old task methods continue to function, except for operations involving comments. Changes are available starting from module version `tasks 25.700.0`.

## What Remains Unchanged

- The [task.*](index.md) methods for creating and updating tasks, files, and checklists work as before.
- Adding a comment via [task.commentitem.add](./comment-item/task-comment-item-add.md) is still functional.

## What Has Changed in Comments {#comments}

- Updating and deleting comments using the methods task.commentitem.update and task.commentitem.delete no longer work. Use the Chat methods instead:
  - [im.message.update](../chats/messages/im-message-update.md) to change the text,
  - [im.message.delete](../chats/messages/im-message-delete.md) to delete a message.
- Retrieving the list of comments via task.commentitem.getlist is no longer functional. Get task chat messages through [im.dialog.messages.get](../chats/messages/im-dialog-messages-get.md).
- Use the method [im.disk.file.commit](../chats/files/im-disk-file-commit.md) to send files in the task chat.
- The chat associated with the task is returned in the response of [tasks.task.get](./tasks-task-get.md). Use its identifier for requests in chat methods.

### How to Get the Task Chat ID via tasks.task.get

#### Old API Version

```http
POST https://{installation_address}/rest/{user_id}/{rest_app_password}/tasks.task.get
{
    "taskId": 51,
    "select": ["CHAT_ID"]
}
```

Example response:

```json
{
    "result": {
        "task": {
            "id": "3835",
            "chatId": 2537,
            "favorite": "N",
            "group": [],
            "action": {
                ...
            }
        }
    }
}    
```

#### New API Version

Request the fields `chat.id`, `chat.entityId`, `chat.entityType` for the task:

```http
POST https://{installation_address}/rest/api/{user_id}/{rest_app_password}/tasks.task.get
{
    "id": 51,
    "select": ["id", "chat.id", "chat.entityId", "chat.entityType"]
}
```

Example response:

```json
{
    "result": {
        "item": {
            "id": 51,
            "chat": {
                "id": 58,
                "entityId": 51,
                "entityType": "TASKS_TASK"
            }
        }
    }
}
```

{% note info "" %}

Starting from module version `tasks 25.700.0`, some methods can be called in the new format.

The new API call differs by the addition of the /api/ parameter in the request.

Old version:

`https://{installation_address}/rest/{user_id}/{rest_app_password}/tasks.task.get`

New version:

`https://{installation_address}/rest/api/{user_id}/{rest_app_password}/tasks.task.get`

Documentation for the new version of the method call is available in OpenApi format. To obtain OpenApi, call the method `documentation`:

`https://{installation_address}/rest/api/{user_id}/{rest_app_password}/documentation`

{% endnote %}

## How to Send Messages to a Task

- Old method [task.commentitem.add](./comment-item/task-comment-item-add.md).
- New method [tasks.task.chat.message.send](../rest-v3/tasks/tasks-task-chat-message-send.md). To send a file in the task chat, use the method [im.disk.file.commit](../chats/files/im-disk-file-commit.md).

## Events

- The event [OnTaskCommentAdd](./comment-item/events-comment/on-task-comment-add.md) works. When working with the new task card, the handler will receive parameters:
    - `MESSAGE_ID` with the identifier of the message in the task chat,
    - `TASK_ID` with the identifier of the task,
    - `'ID' => 0` the identifier of the comment will equal zero.

- The events [OnTaskCommentUpdate](./comment-item/events-comment/on-task-comment-update.md) and [OnTaskCommentDelete](./comment-item/events-comment/on-task-comment-delete.md) do not work in the new task card.

## Task Result

- The method [tasks.task.result.list](./result/tasks-task-result-list.md) works. When working with the new task card, all task results will be returned with the parameter `commentId: 0`.

- The methods [tasks.task.result.addFromComment](./result/tasks-task-result-add-from-comment.md) and [tasks.task.result.deleteFromComment](./result/tasks-task-result-delete-from-comment.md) do not work in the new task card.

## Widgets

The locations of the widgets [TASK_VIEW_SIDEBAR](../widgets/task/view-sidebar.md), [TASK_VIEW_TOP_PANEL](../widgets/task/view-top-panel.md), [TASK_VIEW_TAB](../widgets/task/view-tab.md) are no longer relevant in the new task card. In the new card, all widgets are displayed in a single "Applications" block.

All previously registered widgets continue to work. New widgets can also be registered, and they will be displayed in the "Applications" block.

![Embedded Applications](_images/widget.png)