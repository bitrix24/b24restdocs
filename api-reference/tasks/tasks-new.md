# New Task Card: Comments and Events

The new task card has moved comments to chat. The old task methods continue to work, except for operations with comments. Changes are available from version `tasks 25.700.0`.

## What Remains Unchanged

- The [task.*](index.md) methods for creating and updating tasks, files, and checklists work as before.
- Adding a comment through [task.commentitem.add](./comment-item/task-comment-item-add.md) still works.

## What Has Changed in Comments

- Updating and deleting comments using the methods task.commentitem.update and task.commentitem.delete no longer work. Use the Chat methods:
  - [im.message.update](../chats/messages/im-message-update.md) to change the text,
  - [im.message.delete](../chats/messages/im-message-delete.md) to delete a message.
- Retrieving the list of comments through task.commentitem.getlist is no longer functional. Get task chat messages through [im.dialog.messages.get](../chats/messages/im-dialog-messages-get.md).
- The chat associated with the task is returned in the response of [tasks.task.get](./tasks-task-get.md). Use its identifier for requests in chat methods.

### How to Get Task Chat via tasks.task.get

Request the fields `chat.id`, `chat.entityId`, `chat.entityType` from the task:

```http
POST https://{installation_address}/rest/api/{user_id}/{rest_application_password}/tasks.task.get
{
    "id": 51,
    "select": ["id", "chat.id", "chat.entityId", "chat.entityType"]
}
```
{% note info "" %}

The new API call differs by the addition of the /api/ parameter in the request.

Old version:

`https://{installation_address}/rest/{user_id}/{rest_application_password}/tasks.task.get`

New version:

`https://{installation_address}/rest/api/{user_id}/{rest_application_password}/tasks.task.get`

{% endnote %}

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

## How to Send Messages to a Task

- Old method [task.commentitem.add](./comment-item/task-comment-item-add.md).
- New method [tasks.task.chat.message.send](./tasks-task-chat-message-send.md).

## Events

- Task comment events are temporarily unavailable in the new card.