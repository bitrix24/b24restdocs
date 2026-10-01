# Checklists: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A checklist is a list of steps for a task. Each item in the checklist can be completed separately.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Checklists in Tasks](https://helpdesk.bitrix24.com/open/25865167/)

## Structure of Checklists

A checklist in Bitrix24 is a hierarchical list with a tree-like structure. It is linked to a task.

Checklist items support nesting. The `PARENT_ID` field points to the parent item. The root item's `PARENT_ID` is `0`, and its `TITLE` is the checklist name.

```plaintext
Checklist 1 (431)           ← PARENT_ID=0
├── first item (433)        ← PARENT_ID=431
│   ├── subitem 1 (435)     ← PARENT_ID=433
│   └── subitem 2 (445)     ← PARENT_ID=433
├── second item (447)       ← PARENT_ID=431
└── third item (449)        ← PARENT_ID=431
```

The `SORT_INDEX` field specifies the item position. The smaller the number, the higher the item.

Each item can have the status *completed* `IS_COMPLETE = 'Y'` or *not completed* `IS_COMPLETE = 'N'`. When the status changes, the system automatically fills in the fields *who changed* `TOGGLED_BY` and *when changed* `TOGGLED_DATE`.

An item can be marked as important `IS_IMPORTANT = 'Y'`.

## Linking Checklists to Other Objects

**Task.** Methods accept the task ID in the `TASKID` parameter, and the item data returns it in the `TASK_ID` field. You can obtain the identifier using the [create new task](../tasks-task-add.md) method or the [get task list](../tasks-task-list.md) method.

**User.** A checklist item can have links to users in the fields:

- `TOGGLED_BY` — the identifier of the user who last changed the item status

- `MEMBERS` — an array with information about watchers `"TYPE": "U"` and participants `"TYPE": "A"` in the checklist item

**Files.** A checklist item can contain files. The `ATTACHMENTS` field stores objects with descriptions of each file, including the identifier `FILE_ID`. You can get information about a file by its identifier using the [disk.file.get](../../disk/file/disk-file-get.md) method.

{% note tip "User Documentation" %}

- [Tasks in Bitrix24](https://helpdesk.bitrix24.com/open/18034564/)
- [Create a Task](https://helpdesk.bitrix24.com/open/25865519/)

{% endnote %}

## Getting Started

1. Retrieve the task ID using the [tasks.task.list](../tasks-task-list.md) method.
2. Add items using the [task.checklistitem.add](./task-checklist-item-add.md) method — it returns the item ID.
3. Retrieve the task's items using the [task.checklistitem.getlist](./task-checklist-item-get-list.md) method, or a single item using the [task.checklistitem.get](./task-checklist-item-get.md) method.
4. Update an item using the [task.checklistitem.update](./task-checklist-item-update.md) method, move it using [task.checklistitem.moveafteritem](./task-checklist-item-move-after-item.md), change its completion status using [task.checklistitem.complete](./task-checklist-item-complete.md) and [task.checklistitem.renew](./task-checklist-item-renew.md), or delete it using [task.checklistitem.delete](./task-checklist-item-delete.md). These methods require a `TASKID` and `ITEMID` pair.

All methods in this section accept parameters by position, not by name: the names `TASKID`, `ITEMID`, and `FIELDS` are ignored, so follow the order given in each method's parameter table. Methods respond differently to a nonexistent `ITEMID`: some return an error, others return `false`, `true`, or `null`. Each method's page describes how to handle such a response.

## Check Permissions Before an Action

To find out whether a user is allowed to add, update, delete, or move an item, or change its status, use the [task.checklistitem.isactionallowed](./task-checklist-item-is-action-allowed.md) method.

## Reference Information on Methods

The [task.checklistitem.getmanifest](./task-checklist-item-get-manifest.md) method returns an up-to-date description of the checklist methods. Its response structure may change, so use the method only as a reference.

## Overview of Methods {#all-methods}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the methods: depending on the method

#|
|| **Method** | **Description** ||
|| [task.checklistitem.add](./task-checklist-item-add.md) | Adds a new checklist item to a task ||
|| [task.checklistitem.update](./task-checklist-item-update.md) | Updates a checklist item ||
|| [task.checklistitem.get](./task-checklist-item-get.md) | Gets a checklist item by ID ||
|| [task.checklistitem.getlist](./task-checklist-item-get-list.md) | Gets all checklist items of a task in a single response ||
|| [task.checklistitem.delete](./task-checklist-item-delete.md) | Deletes a checklist item together with its subitems ||
|| [task.checklistitem.moveafteritem](./task-checklist-item-move-after-item.md) | Places a checklist item in the list after the specified one ||
|| [task.checklistitem.complete](./task-checklist-item-complete.md) | Marks a checklist item as completed ||
|| [task.checklistitem.renew](./task-checklist-item-renew.md) | Marks a completed checklist item as not completed ||
|| [task.checklistitem.isactionallowed](./task-checklist-item-is-action-allowed.md) | Checks whether an action is allowed on a checklist item ||
|| [task.checklistitem.getmanifest](./task-checklist-item-get-manifest.md) | Gets a list of methods and their descriptions ||
|#
