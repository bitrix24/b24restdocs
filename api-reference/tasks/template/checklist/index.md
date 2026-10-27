# Task Template Checklists: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Task template checklists allow you to prepare a list of actions in advance for tasks that will be created based on the template.

The `tasks.template.checklist.*` methods work with a task template checklist. They allow you to add items, update them, move them, mark them as completed or return them to work, and add or remove attachments.

For checklists of existing tasks, use the [task.checklistitem.*](../../checklist-item/index.md) method group.

> Quick navigation: [all methods](#all-methods)
>
> User documentation:
> - [Task templates](https://helpdesk.bitrix24.com/open/20883624/)
> - [Checklists in Tasks](https://helpdesk.bitrix24.com/open/25865167/)

## Getting Started

1. Create a task template using the [tasks.template.add](../tasks-template-add.md) method. Pass the template identifier from the response to all `tasks.template.checklist.*` methods in the `templateId` parameter.
2. Create a checklist using the [tasks.template.checklist.add](./tasks-template-checklist-add.md) method: first the root item with `PARENT_ID` = `0`, which is the checklist title, then items with the `PARENT_ID` of the root item.
3. Retrieve the checklist structure and item identifiers using the [tasks.template.checklist.list](./tasks-template-checklist-list.md) method.
4. Update an item using the [tasks.template.checklist.update](./tasks-template-checklist-update.md) method, or delete it together with its subitems using the [tasks.template.checklist.delete](./tasks-template-checklist-delete.md) method.

The order, status, and attachments of items are changed by separate methods, listed in the [overview of methods](#all-methods).

## Relationship with Other Objects

**Task Template.** All `tasks.template.checklist.*` methods work with the checklist of a specific task template. The template identifier is passed in the `templateId` parameter. You can obtain the identifier of the new template when creating it using the [tasks.template.add](../tasks-template-add.md) method. To read template data by identifier, use [tasks.template.get](../tasks-template-get.md).

**Files.** You can attach files from Drive to the task template checklist item using the [tasks.template.checklist.addAttachmentsFromDisk](./tasks-template-checklist-add-attachments-from-disk.md) method. In the `filesIds` parameter, pass an array with the identifiers of the Drive files. Precede each identifier with the prefix `n`, for example: `["n428", "n345"]`. You can obtain file identifiers in two ways.

Use one of the file upload methods:

- [disk.storage.uploadfile](../../../disk/storage/disk-storage-upload-file.md)
- [disk.folder.uploadfile](../../../disk/folder/disk-folder-upload-file.md)

Use one of the methods to get the list of files:

- [disk.storage.getchildren](../../../disk/storage/disk-storage-get-children.md)
- [disk.folder.getchildren](../../../disk/folder/disk-folder-get-children.md)

## Method Specifics

- **Response format of `list`.** The [tasks.template.checklist.list](./tasks-template-checklist-list.md) method returns values as strings and flags as `Y` and `N`. Other methods return numbers and `true` or `false`. The `list` method has no pagination: all items are returned in a single response
- **Attachment identifiers.** In the `attachments` object, the key is the attachment identifier, which is passed in `attachmentsIds` of the [tasks.template.checklist.removeAttachments](./tasks-template-checklist-remove-attachments.md) method. A value such as `n10921` is the identifier of the file on Drive
- **Deleting an item.** The [tasks.template.checklist.delete](./tasks-template-checklist-delete.md) method deletes an item together with all its subitems; the files remain on Drive

## Overview of Methods {#all-methods}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can perform the method: depending on the method

### Core Methods

#| 
|| **Method** | **Description** ||
|| [tasks.template.checklist.add](./tasks-template-checklist-add.md) | Adds a checklist item ||
|| [tasks.template.checklist.update](./tasks-template-checklist-update.md) | Updates a checklist item ||
|| [tasks.template.checklist.get](./tasks-template-checklist-get.md) | Retrieves a checklist item by identifier ||
|| [tasks.template.checklist.list](./tasks-template-checklist-list.md) | Retrieves a list of checklist items ||
|| [tasks.template.checklist.delete](./tasks-template-checklist-delete.md) | Deletes a checklist item ||
|#

### Order and Status

#| 
|| **Method** | **Description** ||
|| [tasks.template.checklist.moveAfter](./tasks-template-checklist-move-after.md) | Moves an item after the specified one ||
|| [tasks.template.checklist.moveBefore](./tasks-template-checklist-move-before.md) | Moves an item before the specified one ||
|| [tasks.template.checklist.complete](./tasks-template-checklist-complete.md) | Marks an item as completed ||
|| [tasks.template.checklist.renew](./tasks-template-checklist-renew.md) | Returns an item to an incomplete state ||
|#

### Attachments

#| 
|| **Method** | **Description** ||
|| [tasks.template.checklist.addAttachmentByContent](./tasks-template-checklist-add-attachment-by-content.md) | Adds an attachment from content ||
|| [tasks.template.checklist.addAttachmentsFromDisk](./tasks-template-checklist-add-attachments-from-disk.md) | Adds attachments from Drive ||
|| [tasks.template.checklist.removeAttachments](./tasks-template-checklist-remove-attachments.md) | Removes attachments from the checklist item ||
|#
