# Drive Files: Overview of Methods

You can store text documents, spreadsheets, presentations, images, and other information on Drive. Users can upload, edit, copy files, and configure access permissions for them.

> Quick navigation: [all methods](#all-methods) 

## How to Manage Files

A new file should be uploaded using the [disk.folder.uploadfile](../folder/disk-folder-upload-file.md) method to a folder by its identifier.   

You can change the location of files within the Drive structure: move them using the [disk.file.moveto](./disk-file-move-to.md) method or copy them to other folders using the [disk.file.copyto](./disk-file-copy-to.md) method.

You can retrieve file field values using the [disk.file.get](./disk-file-get.md) method. For example, to check if a file has been moved to the trash.

You can rename a file using the [disk.file.rename](./disk-file-rename.md) method.

{% note tip "User Documentation" %}

- [Documents Online: Getting Started](https://helpdesk.bitrix24.com/open/20595366/)
- [How to Work with Documents on Bitrix24 Drive](https://helpdesk.bitrix24.com/open/20606316/)
- [How to Lock a Document on Drive](https://helpdesk.bitrix24.com/open/21140222/)

{% endnote %}

## File Versions

You can obtain a list of file versions using the [disk.file.getVersions](./disk-file-get-versions.md) method. This method uses the file identifier as a parameter. 

To get information about a version, use the [disk.version.get](../version/disk-version-get.md) method. This method takes a parameter with the version identifier, not the file identifier.

Drive tools allow you to restore the desired version of a file. This is done using the [disk.file.restoreFromVersion](./disk-file-restore-from-version.md) method.

You can upload a new version of a file using the [disk.file.uploadversion](./disk-file-upload-version.md) method.

{% note tip "User Documentation" %}

- [How Long Are Document Versions Stored on Drive](https://helpdesk.bitrix24.com/open/18874394/)

{% endnote %}

## Access for External Users

To provide access to a file for an external user, you need to create a public link. This will allow you to share the file with people who do not have access to Bitrix24. You can obtain a public link for a file using the [disk.file.getExternalLink](./disk-file-get-external-link.md) method.

{% note tip "User Documentation" %}

- [How to Use Public and Internal Links to Files in Bitrix24](https://helpdesk.bitrix24.com/open/19561436/)

{% endnote %}

## How to Delete Files

Files can be moved to the trash using the [disk.file.markdeleted](./disk-file-mark-deleted.md) method. Deleted files can be restored using the [disk.file.restore](./disk-file-restore.md) method within 30 days. 

To permanently delete a file without the possibility of recovery, you need to use the [disk.file.delete](./disk-file-delete.md) method. This will destroy the file forever. 

{% note tip "User Documentation" %}

- [Trash on Drive in Bitrix24](https://helpdesk.bitrix24.com/open/19646680/)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [disk.file.getfields](./disk-file-get-fields.md) | Returns the description of file fields ||
|| [disk.file.get](./disk-file-get.md) | Returns the file by identifier ||
|| [disk.file.rename](./disk-file-rename.md) | Renames the file ||
|| [disk.file.copyto](./disk-file-copy-to.md) | Copies the file to the specified folder ||
|| [disk.file.moveto](./disk-file-move-to.md) | Moves the file to the specified folder ||
|| [disk.file.delete](./disk-file-delete.md) | Destroys the file forever ||
|| [disk.file.markdeleted](./disk-file-mark-deleted.md) | Moves the file to the trash ||
|| [disk.file.restore](./disk-file-restore.md) | Restores the file from the trash ||
|| [disk.file.uploadversion](./disk-file-upload-version.md) | Uploads a new version of the file ||
|| [disk.file.getVersions](./disk-file-get-versions.md) | Returns the list of file versions ||
|| [disk.file.restoreFromVersion](./disk-file-restore-from-version.md) | Restores the file from a specific version ||
|| [disk.file.getExternalLink](./disk-file-get-external-link.md) | Returns a public link to the file ||
|#