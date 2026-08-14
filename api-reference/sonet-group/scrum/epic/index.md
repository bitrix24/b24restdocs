# Epics in Scrum: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

An epic is a theme, context, or large goal related to a task. Attach tasks to epics to make your backlog clearer.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Team collaboration with Scrum](https://helpdesk.bitrix24.com/open/21300770/)

## Linking Epics to Other Objects

**Group.** Epics are linked to a group (Scrum) by the group identifier `groupId`. You can obtain the identifier using the [create new group](../../sonet-group-create.md) method or the [get group list](../../socialnetwork-api-workgroup-list.md) method. A group is considered a Scrum if the `SCRUM_MASTER_ID` field is filled.

**User.** An epic is associated with users by their numerical identifier in the `createdBy` and `modifiedBy` parameters. You can obtain the user identifier using the [user.get](../../../user/user-get.md) method.

## How to Get Started

1. Retrieve the group identifier using the [socialnetwork.api.workgroup.list](../../socialnetwork-api-workgroup-list.md) method.
2. Create an epic using the [tasks.api.scrum.epic.add](./tasks-api-scrum-epic-add.md) method.
3. Retrieve the Scrum task data using the [tasks.api.scrum.task.get](../task/tasks-api-scrum-task-get.md) method.
4. Link a task to an epic using the [tasks.api.scrum.task.update](../task/tasks-api-scrum-task-update.md) method.

{% note tip "User Documentation" %}

- [Bitrix24 Scrum: Getting started](https://helpdesk.bitrix24.com/open/25787711/)
- [How to create a group and project](https://helpdesk.bitrix24.com/open/22796428/)

{% endnote %}

## How to Attach Files to an Epic

You can attach files from the Disk to an epic. To do this, pass an array of file identifiers in the `files` parameter. Prefix each identifier with `n`, for example: `"files": ["n428", "n345"]`. You can obtain file identifiers in two ways.

Use one of the file upload methods:

- [disk.storage.uploadfile](../../../disk/storage/disk-storage-upload-file.md)
- [disk.folder.uploadfile](../../../disk/folder/disk-folder-upload-file.md)

Use one of the methods to get the list of files:

- [disk.storage.getchildren](../../../disk/storage/disk-storage-get-children.md)
- [disk.folder.getchildren](../../../disk/folder/disk-folder-get-children.md)

## Related Methods

- [{#T}](./tasks-api-scrum-epic-delete.md)

## Overview of Methods {#all-methods}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can perform the method: depending on the method

#|
|| **Method** | **Description** ||
|| [tasks.api.scrum.epic.add](./tasks-api-scrum-epic-add.md) | Adds an epic in Scrum ||
|| [tasks.api.scrum.epic.update](./tasks-api-scrum-epic-update.md) | Updates an epic in Scrum ||
|| [tasks.api.scrum.epic.get](./tasks-api-scrum-epic-get.md) | Retrieves the field values of an epic by its `id` ||
|| [tasks.api.scrum.epic.list](./tasks-api-scrum-epic-list.md) | Retrieves a list of epics ||
|| [tasks.api.scrum.epic.delete](./tasks-api-scrum-epic-delete.md) | Deletes an epic ||
|| [tasks.api.scrum.epic.getFields](./tasks-api-scrum-epic-get-fields.md) | Retrieves available fields of an epic ||
|#
