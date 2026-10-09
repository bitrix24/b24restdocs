# Tasks: typical scenarios

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A scenario describes one practical task and the sequence of methods needed to complete it. These scenarios show how to create a task with a file, attach a file to an existing task, transfer a file from CRM, add a comment with an attachment, link a task to an SPA item, calculate employees' time, and reassign a terminated employee's tasks.

> Quick links: [all scenarios](#choose-tutorial)
>
> User documentation: [Tasks in Bitrix24](https://helpdesk.bitrix24.com/open/18034564/)

## Connection with Other Objects

Scenarios are built around a task and its related objects: Drive files, comments, and CRM. Creating and retrieving tasks is performed by the [tasks.task.*](../../api-reference/tasks/index.md#all-methods) method group.

- **Drive files.** To attach a file to a task, first upload it to Drive with [disk.folder.uploadFile](../../api-reference/disk/folder/disk-folder-upload-file.md). Pass the returned Drive object `ID` in the `UF_TASK_WEBDAV_FILES` field of [tasks.task.add](../../api-reference/tasks/tasks-task-add.md), or in the `fileId` parameter of [tasks.task.files.attach](../../api-reference/tasks/tasks-task-files-attach.md) if the task already exists. The field and method require different value formats
- **Comments.** Task comments are stored in the task chat. To add a comment with a file, get the task `chatId` using the [tasks.task.get](../../api-reference/tasks/tasks-task-get.md) method and send the file together with the message text using [im.v2.File.upload](../../api-reference/chat-bots/chat-bots-v2/im.v2/files/file-upload.md). The [task.commentitem.add](../../api-reference/tasks/comment-item/task-comment-item-add.md) method still adds comments, but its development stopped in module version `tasks 25.700.0`

- **CRM and SPAs.** A task is linked to CRM objects through the `UF_CRM_TASK` field of [tasks.task.add](../../api-reference/tasks/tasks-task-add.md). For an SPA item, use the `SYMBOL_CODE_SHORT_id` format, for example `Tb1_29`: [crm.enum.ownertype](../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) returns the type code, and [crm.item.list](../../api-reference/crm/universal/crm-item-list.md) returns the item's `id`. The [linking scenario](./how-to-connect-task-to-spa.md) describes the steps

## Getting Started

1. Choose a scenario: create a task with a file, attach a file from Drive or CRM, add a comment with a file, link a task to an SPA, calculate time spent, or reassign a terminated employee's tasks
2. Select a scenario in the [How to choose a scenario](#choose-tutorial) table
3. Check which permissions and scopes are specified in the selected scenario
4. Prepare the data listed in the "Prepare" column for the selected scenario
5. Execute the methods in the order described in the scenario

## Important Considerations

- Task methods require the [`task`](../../api-reference/scopes/permissions.md) scope. Uploading a file to Drive also requires `disk`; transferring a file from CRM or linking an SPA requires `crm`. Adding a comment with a file through the task chat requires `im`, but not `disk`. Scenarios involving employees require `user_brief` or `user_basic`, depending on how users are found. Each tutorial lists its exact scopes at the beginning
- Methods run with the permissions of the application or webhook user. For an existing task, check access to the task; for Drive files, check permission to add files to the folder and read the file; for comments, check access to the task chat; for an SPA item, check CRM read permission
- In the `UF_TASK_WEBDAV_FILES` field of [tasks.task.add](../../api-reference/tasks/tasks-task-add.md), pass an array containing the Drive object `ID` with an `n` prefix, for example `["n6687"]`. Without the prefix, the file may not appear in the task. In the `fileId` parameter of [tasks.task.files.attach](../../api-reference/tasks/tasks-task-files-attach.md), pass the same `ID` as a number without `n`, for example `6687`. A prefixed value is invalid for this numeric parameter. In both cases, do not use `FILE_ID` from the Drive response instead of `ID`
- If you pass `entityTypeId` or another type's code instead of `SYMBOL_CODE_SHORT` in `UF_CRM_TASK`, the task may be created without an error while `ufCrmTask` remains empty. This may also happen when the SPA is not linked to tasks. Check the link with [tasks.task.get](../../api-reference/tasks/tasks-task-get.md) by adding `UF_CRM_TASK` to `select`, and check the `linkedUserFields` setting as described in the [linking scenario](./how-to-connect-task-to-spa.md)

## How to choose a scenario {#choose-tutorial}

#|
|| **If You Need To** | **Prepare** | **Open** ||
|| Create a task and attach a Drive file immediately | Drive folder and assignee IDs, file | [How to create a task with an attached file](./how-to-create-task-with-file.md) ||
|| Upload a file to Drive and attach it to an existing task | Task and Drive folder IDs, file | [How to upload a file to a task](./how-to-upload-file-to-task.md) ||
|| Transfer a file from a deal's file field to an existing task | CRM item ID and type, file field code, task and Drive folder IDs | [How to transfer a file from a CRM field to a task](./how-to-transfer-file-from-crm-to-task.md) ||
|| Add a comment with a file through the task chat | Task ID, file | [How to create a comment in a task and attach a file to it](./how-to-create-comment-with-file.md) ||
|| Create a task linked to an SPA item | SPA item, assignee ID, task linking setting | [How to link a task to an SPA](./how-to-connect-task-to-spa.md) ||
|| Calculate time spent on tasks for each employee | Task filter, reporting period, time tracking records | [How to calculate time spent on tasks for each employee](./how-to-calculate-employee-time-by-tasks.md) ||
|| Reassign a terminated employee's unfinished tasks | Two employees' details, `DELEGATE` permission for tasks | [How to Delegate Incomplete Tasks of a Terminated Employee](./how-to-delegate-fired-employee-tasks.md) ||
|| View the full task methods reference | — | [Tasks: methods overview](../../api-reference/tasks/index.md) ||
|#
