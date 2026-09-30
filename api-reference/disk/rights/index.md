# Disk Access Permissions: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Access levels determine user access to folders and files in Drive. The REST API uses them when uploading files and granting access to existing folders.

> Quick Navigation: [All Methods](#all-methods)
>
> User Documentation: [Configure Access Permissions to Personal Drive](https://helpdesk.bitrix24.com/open/25750335/)

## How to Start

1. Get access levels using [disk.rights.getTasks](./disk-rights-get-tasks.md)
2. Select the required access level
3. Pass it when uploading a file or granting access to an existing folder

## Features of Working with Access Permissions

The method [disk.rights.getTasks](./disk-rights-get-tasks.md) returns identifiers for three levels of access:

- read
- edit
- full access

When uploading a file, pass the access level identifier in the `TASK_ID` field of an item in the `rights` array. The `rights` parameter is supported by [disk.storage.uploadFile](../storage/disk-storage-upload-file.md) and [disk.folder.uploadFile](../folder/disk-folder-upload-file.md).

To grant access to an existing folder, use [disk.folder.shareToUser](../folder/disk-folder-share-to-user.md). It accepts the access level name in the `taskName` parameter rather than the `TASK_ID` identifier.

## Overview of Methods {#all-methods}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [disk.rights.getTasks](./disk-rights-get-tasks.md) | Returns a list of available access levels ||
|#
