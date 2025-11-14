# Checklists: Overview of Methods

A checklist is a list of steps for a task. Each item in the checklist can be completed separately.

> Quick navigation: [all methods and events](#all-methods) 
> 
> User documentation: [checklists in tasks](https://helpdesk.bitrix24.com/open/17740572/)

## Structure of Checklists

A checklist in Bitrix24 is a hierarchical list with a tree-like structure of up to three levels. It is linked to a task by `TASK_ID`.

Checklist items support nesting. The `PARENT_ID` field points to the parent item. The root item's `PARENT_ID` is `0`.

The `SORT_INDEX` field determines the position of the item. The smaller the number, the higher the item.

Each item can have a status of *Completed* `IS_COMPLETE = 'Y'` or *Not completed* `IS_COMPLETE = 'N'`. When the status changes, the system automatically fills in the fields *Who changed* `TOGGLED_BY` and *When changed* `TOGGLED_DATE`.

An item can be marked as important `IS_IMPORTANT = 'Y'`.

## Linking Checklists to Other Objects

**Task.** The checklist is linked to the task by the identifier `TASKID`. You can obtain the identifier using the [create new task](../tasks-task-add.md) method or the [get task list](../tasks-task-list.md) method.

**User.** A checklist item can have links to users in the fields:

-  `TOGGLED_BY` — the identifier of the user who completed the item,

-  `MEMBERS` — an array with information about watchers `"TYPE": "U"` and participants `"TYPE": "A"` in the checklist item.

**Files.** A checklist item can contain files. The `ATTACHMENTS` field stores objects with descriptions of each file, including the identifier `FILE_ID`. You can get information about a file by its identifier using the [disk.file.get](../../disk/file/disk-file-get.md) method.

{% note tip "User Documentation" %}

- [Bitrix24 Tasks](https://helpdesk.bitrix24.com/open/18034564/)

{% endnote %}

## Move a Checklist Item

You can change the position of an item using the [task.checklistitem.moveafteritem](./task-checklist-item-move-after-item.md) method. This method moves the `itemId` to a position after the `afterItemId`. Both items must be in the same task `taskId`. The items can be in different sublists, but after moving, `itemId` will have the same `PARENT_ID` as `afterItemId`.

**Example.** Let's place the item with `ID=453` after the item with `ID=447`.

```plaintext
BEFORE:                                        AFTER:
Checklist 1 (431)                             Checklist 1 (431)
├── first item (433)                          ├── first item (433)
│   ├── subitem 1 (435)                        │   ├── subitem 1 (435)
│   ├── subitem 2 (445)                        │   └── subitem 2 (445)
│   └── subitem 3 (453) ← PARENT_ID=433       ├── second item (447)
├── second item (447)                          ├── subitem 3 (453) ← PARENT_ID=431
└── third item (449)                           └── third item (449)
```

## Complete a Checklist Item

The [task.checklistitem.complete](./task-checklist-item-complete.md) method marks a checklist item as completed. The system sets the value `'Y'` in the `IS_COMPLETE` field and automatically fills in the fields:

-  `TOGGLED_BY` — the identifier of the user who changed the item's status,

-  `TOGGLED_DATE` — the current date and time when the user changed the item.

The [task.checklistitem.renew](./task-checklist-item-renew.md) method returns the checklist item to work. The system sets the value `'N'` in the `IS_COMPLETE` field and updates the `TOGGLED_BY` and `TOGGLED_DATE` fields.

## Who Can Add or Change an Entry

To add, change, or delete a checklist item, access permissions are required. You can check permissions using the [task.checklistitem.isactionallowed](./task-checklist-item-is-action-allowed.md) method.

## Reference Information on Methods

You can find up-to-date information about methods for working with checklists in tasks using the [task.checklistitem.getmanifest](./task-checklist-item-get-manifest.md) method. It is recommended to use it only as a reference, as the response structure may change at any time.

## Overview of Methods {#all-methods}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

#|
|| **Method** | **Description** ||
|| [task.checklistitem.add](./task-checklist-item-add.md) | Adds a new checklist item to a task ||
|| [task.checklistitem.update](./task-checklist-item-update.md) | Updates a checklist item ||
|| [task.checklistitem.get](./task-checklist-item-get.md) | Gets a checklist item by `id` ||
|| [task.checklistitem.getlist](./task-checklist-item-get-list.md) | Gets a list of checklist items in a task ||
|| [task.checklistitem.delete](./task-checklist-item-delete.md) | Deletes a checklist item ||
|| [task.checklistitem.moveafteritem](./task-checklist-item-move-after-item.md) | Places a checklist item in the list after the specified one ||
|| [task.checklistitem.complete](./task-checklist-item-complete.md) | Marks a checklist item as completed ||
|| [task.checklistitem.renew](./task-checklist-item-renew.md) | Marks a completed checklist item as not completed ||
|| [task.checklistitem.isactionallowed](./task-checklist-item-is-action-allowed.md) | Checks if the action is allowed for the checklist item ||
|| [task.checklistitem.getmanifest](./task-checklist-item-get-manifest.md) | Gets a list of methods and their descriptions ||
|#