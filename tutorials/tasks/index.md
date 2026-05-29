# Tasks: typical scenarios

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A scenario describes a single practical task and the sequence of methods required to perform it. In these tasks, scenarios show how to create a task, attach a file from Drive to it, add a comment with an attachment, and link a task to an SPA item.

> Quick links: [all scenarios](#choose-tutorial)
>
> User documentation: [Tasks in Bitrix24](https://helpdesk.bitrix24.com/open/18034564/)

## Connection with Other Objects

Scenarios are built around a task and its related objects: Drive files, comments, and CRM. Creating and retrieving tasks is performed by the [tasks.task.*](../../api-reference/tasks/index.md#all-methods) method group.

- **Drive files.** To attach a file to a task, first upload the file to Drive using the [disk.folder.uploadfile](../../api-reference/disk/folder/disk-folder-upload-file.md) method. The method returns a `ID` of the Drive file. When creating a task, pass this identifier into the `UF_TASK_WEBDAV_FILES` field of the [tasks.task.add](../../api-reference/tasks/tasks-task-add.md) method with the `n` prefix, for example `n6687`. If the task has already been created, attach the file using the [tasks.task.files.attach](../../api-reference/tasks/tasks-task-files-attach.md) method: to the `fileId` parameter, pass the `ID` of the file without a prefix, for example `6687`
- **Comments.** The [How to create a comment in a task and attach a file to it](./how-to-create-comment-with-file.md) scenario uses the [task.commentitem.add](../../api-reference/tasks/comment-item/task-comment-item-add.md) method. The method continues to add comments, but its development has been halted since module version `tasks 25.700.0`. For new integrations with a task chat, use [tasks.task.chat.message.send](../../api-reference/rest-v3/tasks/tasks-task-chat-message-send.md), and send files using the [im.disk.file.commit](../../api-reference/chats/files/im-disk-file-commit.md) method

- **CRM and SPAs.** A task is linked to CRM objects via the `UF_CRM_TASK` field of the [tasks.task.add](../../api-reference/tasks/tasks-task-add.md) method. The field stores an array of identifiers of linked CRM objects: leads, deals, contacts, companies, or SPA items.

  For an SPA, the `UF_CRM_TASK` value is composed of `SYMBOL_CODE_SHORT` — the object type code — and the `id` item returned by the [crm.item.list](../../api-reference/crm/universal/crm-item-list.md) method:

  1. Retrieve `entityTypeId` and `SYMBOL_CODE_SHORT` using the [crm.enum.ownertype](../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method
  2. Pass `entityTypeId` into [crm.item.list](../../api-reference/crm/universal/crm-item-list.md) to find the SPA item and retrieve its `id`
  3. Pass the value into `UF_CRM_TASK` in the `SYMBOL_CODE_SHORT_id` format, for example `Tb1_29`, where `Tb1` is the `SYMBOL_CODE_SHORT` value, and `29` is the `id` item

## Getting Started

1. Define the scenario: create a task with a file, attach a file to an existing task, add a comment with a file, or link a task to an SPA
2. Select a scenario in the [How to choose a scenario](#choose-tutorial) table
3. Check which permissions and scopes are specified in the selected scenario
4. Prepare the task, Drive file, user, or CRM item identifiers required for the scenario
5. Execute the methods in the order described in the scenario

## How to choose a scenario {#choose-tutorial}

#|
|| **If necessary** | **Open** ||
|| Create a task and immediately attach a file from the Drive to it | [How to create a task with an attached file](./how-to-create-task-with-file.md) ||
|| Upload a file to the Drive and attach it to an existing task | [How to upload a file to a task](./how-to-upload-file-to-task.md) ||
|| Add a comment to a task using the [task.commentitem.add](../../api-reference/tasks/comment-item/task-comment-item-add.md) method and attach a file to it | [How to create a comment in a task and attach a file to it](./how-to-create-comment-with-file.md) ||
|| Create a task linked to an SPA item | [How to link a task to an SPA](./how-to-connect-task-to-spa.md) ||
|| View the full task methods reference | [Tasks: methods overview](../../api-reference/tasks/index.md) ||
|#
