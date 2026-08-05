# Disk Access Permissions: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Access permissions allow sharing the contents of your personal drive with other users, departments, or groups. Permissions can be assigned to the entire drive, as well as to individual folders or files.

> Quick Navigation: [All Methods](#all-methods)
>
> User Documentation: [Configure Access Permissions to Personal Drive](https://helpdesk.bitrix24.com/open/25750335/)

## How to Start

1. Get access levels using [disk.rights.getTasks](./disk-rights-get-tasks.md)
2. Select the required `TASK_ID`
3. Pass `TASK_ID` when uploading a file or configuring folder access

## Features of Working with Access Permissions

The method [disk.rights.getTasks](./disk-rights-get-tasks.md) returns identifiers for three levels of access:

- read
- edit
- full access

Pass these identifiers as the value of the `TASK_ID` parameter to set permissions when uploading a file. For example, when uploading a file to storage using [disk.storage.uploadFile](../storage/disk-storage-upload-file.md) or to a folder using [disk.folder.uploadFile](../folder/disk-folder-upload-file.md).

## Overview of Methods {#all-methods}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [disk.rights.getTasks](./disk-rights-get-tasks.md) | Returns a list of available access levels ||
|#
