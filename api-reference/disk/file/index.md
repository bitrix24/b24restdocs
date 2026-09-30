# Drive Files: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

You can store text documents, spreadsheets, presentations, images, and other information on Drive. Users can upload, edit, copy files, and configure access permissions for them.

> Quick navigation: [all methods](#all-methods)

## How to Start

1. Retrieve the list of available storages using [disk.storage.getList](../storage/disk-storage-get-list.md). For application storage, use [disk.storage.getForApp](../storage/disk-storage-get-for-app.md)
2. Retrieve the files and folders in the root using [disk.storage.getChildren](../storage/disk-storage-get-children.md). To navigate nested folders, use [disk.folder.getChildren](../folder/disk-folder-get-children.md)
3. Upload the file to the storage root using [disk.storage.uploadFile](../storage/disk-storage-upload-file.md) or to the selected folder using [disk.folder.uploadFile](../folder/disk-folder-upload-file.md)
4. Use the `ID` from the upload response to retrieve file parameters using [disk.file.get](./disk-file-get.md)
5. If necessary, move the file using [disk.file.moveTo](./disk-file-move-to.md), copy it using [disk.file.copyTo](./disk-file-copy-to.md), rename it using [disk.file.rename](./disk-file-rename.md), or delete it using [disk.file.delete](./disk-file-delete.md)

If the file identifier is unknown, find the file by name or text inside the document using [disk.file.search](./disk-file-search.md). The search can be limited to a single storage or folder.

## Response Format and Errors

Methods that retrieve or modify a file return a file object in `result`. The result fields depend on the method.

Common errors in this section are `ERROR_ARGUMENT` for invalid parameters, `ERROR_NOT_FOUND` for a missing file, `ACCESS_DENIED` for insufficient permissions, and `DISK_OBJ_22000` for a name conflict. The exact set of errors is specified on each method page.

## Restrictions and Permissions

- Reading, copying, and obtaining a public link require "Read" permission for the file
- Renaming, moving, and working with the trash require "Edit" permission
- Managing versions requires "Full access" permission
- The POST request size in Bitrix24 cloud is limited to 2 GB; when sending Base64, account for an increase of approximately one-third
- How long files remain in the trash depends on the Bitrix24 settings

{% note tip "User Documentation" %}

- [Documents Online: Getting Started](https://helpdesk.bitrix24.com/open/20595366/)
- [How to Work with Documents on Bitrix24 Drive](https://helpdesk.bitrix24.com/open/20606316/)
- [How to Lock a Document on Drive](https://helpdesk.bitrix24.com/open/21140222/)

{% endnote %}

## File Versions

The `disk.file.*` methods manage the file's version list, while [disk.version.get](../version/disk-version-get.md) returns a specific version by its identifier. Restoring an earlier version creates a new current version and does not delete the history.

{% note tip "User Documentation" %}

- [How Long Are Document Versions Stored on Drive](https://helpdesk.bitrix24.com/open/18874394/)

{% endnote %}

## Access for External Users

The [disk.file.getExternalLink](./disk-file-get-external-link.md) method creates a public link for people without access to Bitrix24. An administrator can disable public links in the Bitrix24 settings.

{% note tip "User Documentation" %}

- [How to Use Public and Internal Links to Files in Bitrix24](https://helpdesk.bitrix24.com/open/19561436/)

{% endnote %}

## Relationship with Other Objects

Files are located in storages and folders, use access permissions, and can have version history.

**Storages.** A storage contains the Drive root folder. Retrieve files in the root using [disk.storage.getChildren](../storage/disk-storage-get-children.md), and upload a file using [disk.storage.uploadFile](../storage/disk-storage-upload-file.md).

**Folders.** A folder contains files and nested folders. [disk.folder.getChildren](../folder/disk-folder-get-children.md) returns their list, while [disk.folder.uploadFile](../folder/disk-folder-upload-file.md) uploads a file to the folder.

**Access permissions.** The access level determines which file operations are available to the user. Retrieve available levels using [disk.rights.getTasks](../rights/disk-rights-get-tasks.md).

**Versions.** Previous states of file contents are stored as versions. Retrieve the list of versions using [disk.file.getVersions](./disk-file-get-versions.md), and data for one version using [disk.version.get](../version/disk-version-get.md).

## Deleting Files

The [disk.file.markDeleted](./disk-file-mark-deleted.md) method moves a file to the trash, while [disk.file.restore](./disk-file-restore.md) restores it while it remains in the trash. The [disk.file.delete](./disk-file-delete.md) method deletes a file permanently.

{% note tip "User Documentation" %}

- [Trash on Drive in Bitrix24](https://helpdesk.bitrix24.com/open/19646680/)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [disk.file.getFields](./disk-file-get-fields.md) | Returns the description of file fields ||
|| [disk.file.get](./disk-file-get.md) | Returns the file by identifier ||
|| [disk.file.search](./disk-file-search.md) | Finds files and folders by a text query ||
|| [disk.file.rename](./disk-file-rename.md) | Renames the file ||
|| [disk.file.copyTo](./disk-file-copy-to.md) | Copies the file to the specified folder ||
|| [disk.file.moveTo](./disk-file-move-to.md) | Moves the file to the specified folder ||
|| [disk.file.delete](./disk-file-delete.md) | Deletes the file forever ||
|| [disk.file.markDeleted](./disk-file-mark-deleted.md) | Moves the file to the trash ||
|| [disk.file.restore](./disk-file-restore.md) | Restores the file from the trash ||
|| [disk.file.uploadVersion](./disk-file-upload-version.md) | Uploads a new version of the file ||
|| [disk.file.getVersions](./disk-file-get-versions.md) | Returns the list of file versions ||
|| [disk.file.restoreFromVersion](./disk-file-restore-from-version.md) | Restores the file from a specific version ||
|| [disk.file.getExternalLink](./disk-file-get-external-link.md) | Returns a public link to the file ||
|#
