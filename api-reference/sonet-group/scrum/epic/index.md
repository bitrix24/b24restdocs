# Epics in Scrum: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

An epic is a theme or large goal that Scrum tasks belong to, for example, "User Registration" or "Product Catalog". With epics, the team can:

- group backlog and sprint tasks by a common goal
- assign a color to a theme
- store a description and files shared by all tasks in a theme

Epics work within Scrum. For how Scrum is organized and how to start working with it, see the [Overview of Scrum Methods](../index.md).

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Team collaboration with Scrum](https://helpdesk.bitrix24.com/open/21300770/)

## Linking Epics to Other Objects

An epic is linked to Scrum, Scrum tasks, and users.

**Scrum.** An epic belongs to a single Scrum — a group with the `SCRUM_MASTER_ID` field filled. The Scrum identifier is passed in the `groupId` field. You can find Scrums using the [socialnetwork.api.workgroup.list](../../socialnetwork-api-workgroup-list.md) method with the parameters `filter: {"!SCRUM_MASTER_ID": false}` and `select: ["ID", "NAME", "SCRUM_MASTER_ID"]`: in the response, a Scrum has the `scrumMasterId` field filled, and its `id` is the `groupId`.

**Task.** A Scrum task is attached to an epic through the `epicId` field of the [tasks.api.scrum.task.update](../task/tasks-api-scrum-task-update.md) method. To detach a task, pass `epicId: 0`.

**User.** The `createdBy` and `modifiedBy` fields contain the identifiers of the users who created and last modified the epic. You can retrieve the user identifier using the [user.get](../../../user/user-get.md) method.

## How to Get Started

1. Find a Scrum using the [socialnetwork.api.workgroup.list](../../socialnetwork-api-workgroup-list.md) method and note its `id`
2. Create an epic using the [tasks.api.scrum.epic.add](./tasks-api-scrum-epic-add.md) method: pass the Scrum `groupId` and the `name`. The epic identifier is returned in `result.id`
3. Prepare a task in the same Scrum: when creating the task with the [tasks.task.add](../../../tasks/tasks-task-add.md) method, pass the Scrum identifier in `GROUP_ID`. For how to add a task to the backlog or a sprint, see the [Overview of Scrum Task Methods](../task/index.md)
4. Attach the task to the epic using the [tasks.api.scrum.task.update](../task/tasks-api-scrum-task-update.md) method: pass the task identifier in `id` and the epic identifier in `fields.epicId`. If the task is not yet in the backlog or a sprint, also pass the backlog or sprint `entityId` and `createdBy` in `fields` — without `createdBy`, the method returns the `Item not created` error
5. Check the result using the [tasks.api.scrum.task.get](../task/tasks-api-scrum-task-get.md) method: the `epicId` field in the response matches the epic identifier

{% note tip "User Documentation" %}

- [Bitrix24 Scrum: Getting started](https://helpdesk.bitrix24.com/open/25787711/)
- [How to create a group and project](https://helpdesk.bitrix24.com/open/22796428/)

{% endnote %}

## Epic Data

The [tasks.api.scrum.epic.add](./tasks-api-scrum-epic-add.md) and [tasks.api.scrum.epic.update](./tasks-api-scrum-epic-update.md) methods return the epic object in `result`, and the [tasks.api.scrum.epic.list](./tasks-api-scrum-epic-list.md) method returns an array of such objects. In `list`, fields not included in `select` are returned with the value `0` or an empty string.

Example of an epic object right after creation — `modifiedBy` of a new epic is `0`:

```json
{
    "id": 2,
    "groupId": 2,
    "name": "User Registration",
    "description": "Login form and password recovery",
    "createdBy": 1,
    "modifiedBy": 0,
    "color": "#69dafc"
}
```

Only the [tasks.api.scrum.epic.get](./tasks-api-scrum-epic-get.md) method returns attached files: the `files.VALUE` field contains the identifiers of file attachments. The [disk.attachedObject.get](../../../disk/attached-object/disk-attached-object-get.md) method returns the file name and download link for such an identifier.

## How to Attach Files to an Epic

You can attach Drive files to an epic. Pass an array of file identifiers with the `n` prefix in `fields.files` of the [tasks.api.scrum.epic.add](./tasks-api-scrum-epic-add.md) or [tasks.api.scrum.epic.update](./tasks-api-scrum-epic-update.md) method, for example `"files": ["n428", "n345"]`. The methods skip an identifier without the prefix and a nonexistent file without an error.

Use the `ID` of the object with `TYPE: "file"` from the response of the Drive methods:

- uploading a file: [disk.storage.uploadfile](../../../disk/storage/disk-storage-upload-file.md) or [disk.folder.uploadfile](../../../disk/folder/disk-folder-upload-file.md)
- retrieving a list of files: [disk.storage.getchildren](../../../disk/storage/disk-storage-get-children.md) or [disk.folder.getchildren](../../../disk/folder/disk-folder-get-children.md). These methods return both files and folders

The [tasks.api.scrum.epic.update](./tasks-api-scrum-epic-update.md) method adds new files to those already attached, and an empty `files` array detaches all files.

## Method Access

The [tasks.api.scrum.epic.add](./tasks-api-scrum-epic-add.md), [tasks.api.scrum.epic.update](./tasks-api-scrum-epic-update.md), [tasks.api.scrum.epic.get](./tasks-api-scrum-epic-get.md), and [tasks.api.scrum.epic.delete](./tasks-api-scrum-epic-delete.md) methods work if the user has access to the tasks of the Scrum group. Without such access, the methods return `Access denied`.

The [tasks.api.scrum.epic.list](./tasks-api-scrum-epic-list.md) method returns epics only from the groups the user is a member of. The [tasks.api.scrum.epic.getFields](./tasks-api-scrum-epic-get-fields.md) method returns the field descriptions without checking access to a specific Scrum.

For how to authorize requests, see [Authorization in REST](../../../../settings/how-to-call-rest-api/authorization.md); for request rate limits, see [REST API Limits](../../../../settings/performance/limits.md).

## Important Considerations

- Deleting an epic does not detach its tasks: they keep the `epicId` of the deleted epic. Before [Deleting](./tasks-api-scrum-epic-delete.md) an epic, detach its tasks with `epicId: 0`
- In the `filter` of the [tasks.api.scrum.epic.list](./tasks-api-scrum-epic-list.md) method, field names are written in uppercase, for example `GROUP_ID`. The method does not recognize the `groupId` field and returns an empty list without an error
- The [tasks.api.scrum.epic.list](./tasks-api-scrum-epic-list.md) method returns up to 50 epics per call and does not return `total` or `next`. Request the next page with the `start` parameter increased by 50

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
