# Drive Folders: Overview of Methods

Folders in Bitrix24 Drive allow you to create a logical structure for storing files, such as by document types, dates, or clients. This makes it easier to search for and access the necessary information.

> Quick navigation: [all methods](#all-methods) 

## Folder Structure

Folders in Drive are organized hierarchically. Each folder can contain nested folders and files. You can retrieve the list of files and subfolders using the [disk.folder.getchildren](./disk-folder-get-children.md) method.

A new folder can be created using the [disk.folder.addsubfolder](./disk-folder-add-subfolder.md) method, and a file can be uploaded using the [disk.folder.uploadfile](./disk-folder-upload-file.md) method.

Parent and child folders are linked through the `PARENT_ID` parameter. You can obtain it using the [disk.folder.get](./disk-folder-get.md) method. In addition to `PARENT_ID`, the method will return all folder parameters by the `id` identifier. 

## Folder Operations

You can perform the following operations with Drive folders:

- move within the structure using the [disk.folder.moveto](./disk-folder-move-to.md) method 
- copy to other Drive folders using the [disk.folder.copyto](./disk-folder-copy-to.md) method
- rename using the [disk.folder.rename](./disk-folder-rename.md) method

## External User Access

To provide access to an external user for a folder, you need to create a public link. This will allow you to share the folder's contents with people who do not have access to Bitrix24. You can obtain a public link for the folder using the [disk.folder.getexternallink](./disk-folder-get-external-link.md) method.

## How to Delete Folders

Folders can be moved to the trash using the [disk.folder.markdeleted](./disk-folder-mark-deleted.md) method. Deleted folders can be restored using the [disk.folder.restore](./disk-folder-restore.md) method within 30 days. 

To permanently delete a folder without the possibility of recovery, you need to use the [disk.folder.deletetree](./disk-folder-delete-tree.md) method. This will destroy the folder along with all nested folders and files forever.

{% note tip "User Documentation" %}

- [Trash in Bitrix24 Drive](https://helpdesk.bitrix24.com/open/19646680/)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can perform the method: any user

#|
|| **Method** | **Description** ||
|| [disk.folder.getfields](./disk-folder-get-fields.md) | Returns the description of folder fields ||
|| [disk.folder.get](./disk-folder-get.md) | Returns the folder by identifier ||
|| [disk.folder.getchildren](./disk-folder-get-children.md) | Returns the list of files and folders located in the folder ||
|| [disk.folder.addsubfolder](./disk-folder-add-subfolder.md) | Creates a subfolder ||
|| [disk.folder.copyto](./disk-folder-copy-to.md) | Copies the folder to the specified folder ||
|| [disk.folder.moveto](./disk-folder-move-to.md) | Moves the folder to the specified folder ||
|| [disk.folder.rename](./disk-folder-rename.md) | Renames the folder ||
|| [disk.folder.deletetree](./disk-folder-delete-tree.md) | Permanently deletes the folder and all its contents ||
|| [disk.folder.markdeleted](./disk-folder-mark-deleted.md) | Moves the folder to the trash ||
|| [disk.folder.restore](./disk-folder-restore.md) | Restores the folder from the trash ||
|| [disk.folder.uploadfile](./disk-folder-upload-file.md) | Uploads a new file to the specified folder ||
|| [disk.folder.getexternallink](./disk-folder-get-external-link.md) | Returns a public link to the folder ||
|#