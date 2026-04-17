# Task Template Checklists: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Task template checklists allow you to prepare a list of actions in advance for tasks that will be created based on the template.

The methods in this section work with the task template checklist. They enable you to add items, modify them, rearrange them, mark them as completed or return them to work, as well as add and remove attachments.

For checklists of existing tasks, use the methods from the [Task Checklists](../../checklist-item/index.md) section.

> Quick navigation: [all methods and events](#all-methods) 
> 
> User documentation: [checklists in tasks](https://helpdesk.bitrix24.com/open/25865167/)

## Getting Started

1. Create a task template using the [tasks.template.add](../tasks-template-add.md) method. The identifier of the new template can be obtained in the response of the method. It is used in all methods of the section in the `templateId` parameter.
2. Create the checklist structure using the [tasks.template.checklist.add](./tasks-template-checklist-add.md) method. If you need to create a nested item, pass the parent item's identifier in the `PARENT_ID` field.
3. Retrieve information about a checklist item using the [tasks.template.checklist.get](./tasks-template-checklist-get.md) method if you need to check its parameters. Use this when you need to get a single item by its known `checkListItemId`.
4. Get the list of items using the [tasks.template.checklist.list](./tasks-template-checklist-list.md) method if you need to check the checklist structure and obtain item identifiers.
5. Modify the content of an item using the [tasks.template.checklist.update](./tasks-template-checklist-update.md) method if you need to adjust the name, description, or other parameters.
6. Change the order of items using the [tasks.template.checklist.moveAfter](./tasks-template-checklist-move-after.md) and [tasks.template.checklist.moveBefore](./tasks-template-checklist-move-before.md) methods if you need to rearrange an item relative to another item.
7. Change the status of an item using the [tasks.template.checklist.complete](./tasks-template-checklist-complete.md) and [tasks.template.checklist.renew](./tasks-template-checklist-renew.md) methods if you need to mark an item as completed or return it to work.
8. Add attachments using the [tasks.template.checklist.addAttachmentByContent](./tasks-template-checklist-add-attachment-by-content.md) and [tasks.template.checklist.addAttachmentsFromDisk](./tasks-template-checklist-add-attachments-from-disk.md) methods if the item needs to contain files.
9. If attachments are no longer needed, remove them using the [tasks.template.checklist.removeAttachments](./tasks-template-checklist-remove-attachments.md) method.
10. If an item is no longer needed, delete it using the [tasks.template.checklist.delete](./tasks-template-checklist-delete.md) method.

## Relationship with Other Objects

**Task Template.** All methods in this section work with the checklist of a specific task template. The template identifier is passed in the `templateId` parameter. You can obtain the identifier of the new template when creating it using the [tasks.template.add](../tasks-template-add.md) method. To read template data by identifier, use [tasks.template.get](../tasks-template-get.md).

**Files.** You can attach files from Drive to the task template checklist item using the [tasks.template.checklist.addAttachmentsFromDisk](./tasks-template-checklist-add-attachments-from-disk.md) method. In the `filesIds` parameter, pass an array with the identifiers of the Drive files. Precede each identifier with the prefix `n`, for example: `["n428", "n345"]`. You can obtain file identifiers in two ways.

Use one of the file upload methods:
  - [disk.storage.uploadfile](../../../disk/storage/disk-storage-upload-file.md)
  - [disk.folder.uploadfile](../../../disk/folder/disk-folder-upload-file.md)

Use one of the methods to get the list of files:
  - [disk.storage.getchildren](../../../disk/storage/disk-storage-get-children.md)
  - [disk.folder.getchildren](../../../disk/folder/disk-folder-get-children.md)

{% note tip "User Documentation" %}

- [How to Work with Task Templates](https://helpdesk.bitrix24.com/open/20883624/)

{% endnote %}

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