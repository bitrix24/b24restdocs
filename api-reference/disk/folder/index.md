# Drive Folders: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Folders in Bitrix24 Drive allow you to create a logical structure for storing files, such as by document types, dates, or clients. This makes it easier to search for and access the necessary information.

> Quick navigation: [all methods](#all-methods)

## How to Start

1. Get the storage using the [disk.storage.getList](../storage/disk-storage-get-list.md) method
2. Get root folders using the [disk.storage.getChildren](../storage/disk-storage-get-children.md) method
3. Create a subfolder using the [disk.folder.addSubFolder](./disk-folder-add-subfolder.md) method
4. Upload files using the [disk.folder.uploadFile](./disk-folder-upload-file.md) method

## Folder Structure

Folders in Drive are organized hierarchically. Each folder can contain nested folders and files. You can retrieve the list of files and subfolders using the [disk.folder.getChildren](./disk-folder-get-children.md) method.

A new folder can be created using the [disk.folder.addSubFolder](./disk-folder-add-subfolder.md) method, and a file can be uploaded using the [disk.folder.uploadFile](./disk-folder-upload-file.md) method.

Parent and child folders are linked through the `PARENT_ID` parameter. You can obtain it using the [disk.folder.get](./disk-folder-get.md) method. In addition to `PARENT_ID`, the method will return all folder parameters by the `id` identifier. 

## Folder Operations

You can perform the following operations with Drive folders:

- assign access permissions using the [disk.folder.shareToUser](./disk-folder-share-to-user.md) method
- move within the structure using the [disk.folder.moveTo](./disk-folder-move-to.md) method
- copy to other Drive folders using the [disk.folder.copyTo](./disk-folder-copy-to.md) method
- rename using the [disk.folder.rename](./disk-folder-rename.md) method

## External User Access

To provide an external user with access to a folder, create a public link. This allows you to share the folder contents with people who do not have access to Bitrix24. The [disk.folder.getExternalLink](./disk-folder-get-external-link.md) method returns an existing public link or creates a new one.

## How to Delete Folders

Folders can be moved to the trash using [disk.folder.markDeleted](./disk-folder-mark-deleted.md). Deleted folders can be restored using [disk.folder.restore](./disk-folder-restore.md) while they remain in the trash. The retention period depends on the Bitrix24 settings.

To permanently delete a folder without the possibility of recovery, you need to use the [disk.folder.deleteTree](./disk-folder-delete-tree.md) method. This will destroy the folder along with all nested folders and files forever.

{% note tip "User Documentation" %}

- [Trash in Bitrix24 Drive](https://helpdesk.bitrix24.com/open/19646680/)

{% endnote %}

## Relationship with Other Objects

**Storages.** A folder is located in a Drive storage and is linked to it through the `STORAGE_ID` field. Retrieve the identifier of a folder in the storage root using [disk.storage.getChildren](../storage/disk-storage-get-children.md) or [disk.storage.addFolder](../storage/disk-storage-add-folder.md). All storage methods are listed in the [Drive Storages](../storage/index.md) overview.

**Files.** A folder contains files and nested folders. [disk.folder.getChildren](./disk-folder-get-children.md) returns their list, while [disk.folder.uploadFile](./disk-folder-upload-file.md) uploads a file to the folder by its `ID`. Other file operations are listed in the [Drive Files](../file/index.md) overview.

**Access permissions.** When creating a folder or uploading a file, you can pass a `rights` array with the access level identifier `TASK_ID`. Retrieve available `TASK_ID` values using [disk.rights.getTasks](../rights/disk-rights-get-tasks.md). Access level specifics are described in the [Drive Access Permissions](../rights/index.md) overview.

## Overview of Methods {#all-methods}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the methods: any user

#|
|| **Method** | **Description** ||
|| [disk.folder.getFields](./disk-folder-get-fields.md) | Returns the description of folder fields ||
|| [disk.folder.get](./disk-folder-get.md) | Returns the folder by identifier ||
|| [disk.folder.getChildren](./disk-folder-get-children.md) | Returns a list of files and folders located in the folder ||
|| [disk.folder.addSubFolder](./disk-folder-add-subfolder.md) | Creates a subfolder ||
|| [disk.folder.shareToUser](./disk-folder-share-to-user.md) | Assigns access permissions to the folder ||
|| [disk.folder.copyTo](./disk-folder-copy-to.md) | Copies the folder to the specified folder ||
|| [disk.folder.moveTo](./disk-folder-move-to.md) | Moves the folder to the specified folder ||
|| [disk.folder.rename](./disk-folder-rename.md) | Renames the folder ||
|| [disk.folder.deleteTree](./disk-folder-delete-tree.md) | Permanently deletes the folder and all its contents ||
|| [disk.folder.markDeleted](./disk-folder-mark-deleted.md) | Moves the folder to the trash ||
|| [disk.folder.restore](./disk-folder-restore.md) | Restores the folder from the trash ||
|| [disk.folder.uploadFile](./disk-folder-upload-file.md) | Uploads a new file to the specified folder ||
|| [disk.folder.getExternalLink](./disk-folder-get-external-link.md) | Returns a public link to the folder ||
|#
