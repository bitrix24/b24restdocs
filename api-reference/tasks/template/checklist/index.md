# Task Template Checklists: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Task template checklists allow you to prepare a list of actions in advance for tasks that will be created based on the template.

The `tasks.template.checklist.*` methods work with a task template checklist. They allow you to add items, update them, move them, mark them as completed or return them to work, and add or remove attachments.

Two API versions work with template checklists. Methods of the previous version are called at `/rest/`, and REST 3.0 methods are called at `/rest/api/`: their fields are in camelCase, and a separate `tasks.template.checklist.item.*` group works with a single item. Some method names are the same in both versions, but the behavior differs: `tasks.template.checklist.get` returns one item in the previous version and the entire checklist in REST 3.0. The `complete` and `renew` methods change one item in the previous version and all items at once in REST 3.0.

For checklists of existing tasks, use the [task.checklistitem.*](../../checklist-item/index.md) method group.

> Quick navigation: [all methods](#all-methods) | [REST 3.0 methods](#rest-v3)
>
> User documentation:
> - [Task templates](https://helpdesk.bitrix24.com/open/20883624/)
> - [Checklists in Tasks](https://helpdesk.bitrix24.com/open/25865167/)

## Getting Started

1. Create a task template using the [tasks.template.add](../tasks-template-add.md) or [tasks.template.add](../tasks-template-add-rest-v3.md) v 3.0 method. Pass the template identifier from the response to all checklist methods in the `templateId` parameter.
2. Create a checklist. First add the root item, which is the checklist title, then items with the root item identifier as the parent. In the previous version, use [tasks.template.checklist.add](./tasks-template-checklist-add.md) with `PARENT_ID`; in REST 3.0, use [tasks.template.checklist.item.add](./tasks-template-checklist-item-add-rest-v3.md) v 3.0 with `parentId`. The [tasks.template.checklist.save](./tasks-template-checklist-save-rest-v3.md) v 3.0 method retains the entire checklist in a single call.
3. Retrieve the checklist structure and item identifiers using the [tasks.template.checklist.list](./tasks-template-checklist-list.md) or [tasks.template.checklist.get](./tasks-template-checklist-get-rest-v3.md) v 3.0 method.
4. Update an item using the [tasks.template.checklist.update](./tasks-template-checklist-update.md) or [tasks.template.checklist.item.update](./tasks-template-checklist-item-update-rest-v3.md) v 3.0 method. Delete an item together with its subitems using the [tasks.template.checklist.delete](./tasks-template-checklist-delete.md) or [tasks.template.checklist.item.delete](./tasks-template-checklist-item-delete-rest-v3.md) v 3.0 method.

The order, status, and attachments of items are changed by separate methods, listed in the [overview of methods](#all-methods) and in the [REST 3.0 Methods](#rest-v3) section.

## Relationship with Other Objects

**Task Template.** All checklist methods work with the checklist of a specific task template. The template identifier is passed in the `templateId` parameter. You can obtain the identifier of the new template when creating it using the [tasks.template.add](../tasks-template-add.md) or [tasks.template.add](../tasks-template-add-rest-v3.md) v 3.0 method. To read template data by identifier, use [tasks.template.get](../tasks-template-get.md) or [tasks.template.get](../tasks-template-get-rest-v3.md) v 3.0.

**Participants.** In REST 3.0, you can specify participants and observers of a checklist item in the `accomplices` and `auditors` fields. These users are also added to the template participants. User identifiers can be obtained using the [user.get](../../../user/user-get.md) and [user.search](../../../user/user-search.md) methods.

**Files.** You can attach files from Drive to the task template checklist item. In the previous version, use the [tasks.template.checklist.addAttachmentsFromDisk](./tasks-template-checklist-add-attachments-from-disk.md) method: in the `filesIds` parameter, pass an array with the identifiers of the Drive files and precede each identifier with the prefix `n`, for example: `["n428", "n345"]`. In REST 3.0, use the [tasks.template.checklist.item.attachment.attach](./tasks-template-checklist-item-attachment-attach-rest-v3.md) v 3.0 method: in `fileIds`, pass the file identifiers without a prefix. You can obtain file identifiers in two ways.

Use one of the file upload methods:

- [disk.storage.uploadfile](../../../disk/storage/disk-storage-upload-file.md)
- [disk.folder.uploadfile](../../../disk/folder/disk-folder-upload-file.md)

Use one of the methods to get the list of files:

- [disk.storage.getchildren](../../../disk/storage/disk-storage-get-children.md)
- [disk.folder.getchildren](../../../disk/folder/disk-folder-get-children.md)

To upload a file directly to an item, use the [tasks.template.checklist.addAttachmentByContent](./tasks-template-checklist-add-attachment-by-content.md) or [tasks.template.checklist.item.attachment.upload](./tasks-template-checklist-item-attachment-upload-rest-v3.md) v 3.0 method.

## Method Specifics

### Previous Version

- **Response format of `list`.** The [tasks.template.checklist.list](./tasks-template-checklist-list.md) method returns values as strings and flags as `Y` and `N`. Other methods return numbers and `true` or `false`. The `list` method has no pagination: all items are returned in a single response
- **Attachment identifiers.** In the `attachments` object, the key is the attachment identifier, which is passed in `attachmentsIds` of the [tasks.template.checklist.removeAttachments](./tasks-template-checklist-remove-attachments.md) method. A value such as `n10921` is the identifier of the file on Drive
- **Deleting an item.** The [tasks.template.checklist.delete](./tasks-template-checklist-delete.md) method deletes an item together with all its subitems; the files remain on Drive

### REST 3.0

- **Retaining the entire checklist.** The [tasks.template.checklist.save](./tasks-template-checklist-save-rest-v3.md) method deletes items that are not in the passed list. An empty list clears the checklist. This method does not change attachments of existing items — use the `tasks.template.checklist.item.attachment.*` methods for them
- **Item order.** The [tasks.template.checklist.get](./tasks-template-checklist-get-rest-v3.md) method returns items as a flat list. The tree is built by `parentId`: the value `0` or `null` means a root item
- **Moving.** The [tasks.template.checklist.item.move](./tasks-template-checklist-item-move-rest-v3.md) method sets the item position by the parent and the `sortIndex` number, not by a neighboring item as the methods of the previous version do
- **Detaching files.** A file attached from Drive remains on Drive after it is detached. A file uploaded by the [tasks.template.checklist.item.attachment.upload](./tasks-template-checklist-item-attachment-upload-rest-v3.md) method is deleted when detached
- **Item of another template.** If you pass the identifier of an item that does not belong to the template from `templateId`, the method returns a "not found" error

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

## REST 3.0 Methods {#rest-v3}

> Scope: [`tasks`](../../../scopes/permissions.md)
>
> Who can perform the method: depending on the method

The methods are called at an address with `/rest/api/`. Call specifics and the response format are described in the [REST 3.0 overview](../../../rest-v3.md).

#|
|| **Method** | **Description** ||
|| [tasks.template.checklist.get](./tasks-template-checklist-get-rest-v3.md) | Retrieves all checklist items of a template ||
|| [tasks.template.checklist.save](./tasks-template-checklist-save-rest-v3.md) | Retains the entire template checklist ||
|| [tasks.template.checklist.complete](./tasks-template-checklist-complete-rest-v3.md) | Marks all items as completed ||
|| [tasks.template.checklist.renew](./tasks-template-checklist-renew-rest-v3.md) | Returns all items to an incomplete state ||
|| [tasks.template.checklist.item.add](./tasks-template-checklist-item-add-rest-v3.md) | Adds an item ||
|| [tasks.template.checklist.item.update](./tasks-template-checklist-item-update-rest-v3.md) | Updates an item ||
|| [tasks.template.checklist.item.delete](./tasks-template-checklist-item-delete-rest-v3.md) | Deletes an item ||
|| [tasks.template.checklist.item.move](./tasks-template-checklist-item-move-rest-v3.md) | Moves an item ||
|| [tasks.template.checklist.item.complete](./tasks-template-checklist-item-complete-rest-v3.md) | Marks an item as completed ||
|| [tasks.template.checklist.item.renew](./tasks-template-checklist-item-renew-rest-v3.md) | Returns an item to an incomplete state ||
|| [tasks.template.checklist.item.collapse](./tasks-template-checklist-item-collapse-rest-v3.md) | Collapses an item ||
|| [tasks.template.checklist.item.expand](./tasks-template-checklist-item-expand-rest-v3.md) | Expands an item ||
|| [tasks.template.checklist.item.attachment.attach](./tasks-template-checklist-item-attachment-attach-rest-v3.md) | Attaches Drive files to an item ||
|| [tasks.template.checklist.item.attachment.upload](./tasks-template-checklist-item-attachment-upload-rest-v3.md) | Uploads a file to an item ||
|| [tasks.template.checklist.item.attachment.detach](./tasks-template-checklist-item-attachment-detach-rest-v3.md) | Detaches files from an item ||
|#
