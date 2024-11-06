# Drive Folders: Overview of Methods

Folders in Bitrix24 Drive allow you to create a logical structure for storing files, such as by document types, dates, or clients. This makes it easier to search for and access the necessary information.

> Quick navigation: [all methods](#all-methods) 

## Folder Structure

Folders in Drive are organized hierarchically. Each folder can contain nested folders and files. You can retrieve the list of files and subfolders using the method [disk.folder.getchildren](./disk-folder-get-children.md).

A new folder can be created using the method [disk.folder.addsubfolder](./disk-folder-add-subfolder.md), and a file can be uploaded using the method [disk.folder.uploadfile](./disk-folder-upload-file.md).

Parent and child folders are linked through the `PARENT_ID` parameter. You can obtain it using the method [disk.folder.get](./disk-folder-get.md). In addition to `PARENT_ID`, the method will return all folder parameters by the identifier `id`. 

## Operations with Folders

You can perform the following operations with Drive folders:

- move within the structure using the method [disk.folder.moveto](./disk-folder-move-to.md) 
- copy to other Drive folders using the method [disk.folder.copyto](./disk-folder-copy-to.md)
- rename using the method [disk.folder.rename](./disk-folder-rename.md)

## Access for External Users

To grant access to an external user for a folder, you need to create a public link. This will allow you to share the folder's contents with people who do not have access to Bitrix24. A public link for the folder can be obtained using the method [disk.folder.getExternalLink](./disk-folder-get-external-link.md).

## How to Delete Folders

Folders can be moved to the trash using the method [disk.folder.markdeleted](./disk-folder-mark-deleted.md). Deleted folders can be restored using the method [disk.folder.restore](./disk-folder-restore.md) within 30 days. 

To permanently delete a folder without the possibility of recovery, you need to use the method [disk.folder.deletetree](./disk-folder-delete-tree.md). This will destroy the folder along with all its nested folders and files forever.

{% note tip "User Documentation" %}

- [Trash in Bitrix24 Drive](https://helpdesk.bitrix24.com/open/19646680/)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [disk.folder.getfields](./disk-folder-get-fields.md) | Returns the description of folder fields ||
|| [disk.folder.get](./disk-folder-get.md) | Returns the folder by identifier ||
|| [disk.folder.getchildren](./disk-folder-get-children.md) | Returns a list of files and folders that are directly in the folder ||
|| [disk.folder.addsubfolder](./disk-folder-add-subfolder.md) | Creates a subfolder ||
|| [disk.folder.copyto](./disk-folder-copy-to.md) | Copies the folder to the specified folder ||
|| [disk.folder.moveto](./disk-folder-move-to.md) | Moves the folder to the specified folder ||
|| [disk.folder.rename](./disk-folder-rename.md) | Renames the folder ||
|| [disk.folder.deletetree](./disk-folder-delete-tree.md) | Permanently destroys the folder and all its child elements ||
|| [disk.folder.markdeleted](./disk-folder-mark-deleted.md) | Moves the folder to the trash ||
|| [disk.folder.restore](./disk-folder-restore.md) | Restores the folder from the trash ||
|| [disk.folder.uploadfile](./disk-folder-upload-file.md) | Uploads a new file to the specified folder ||
|| [disk.folder.getExternalLink](./disk-folder-get-external-link.md) | Returns a public link ||
|#