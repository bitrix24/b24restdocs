# Drive Storage: Overview of Methods

Storage is the Drive in Bitrix24 where you can store documents and files, create folders, and retrieve lists of contents.

> Quick navigation: [all methods](#all-methods)

## Types of Storage

In Bitrix24, there are three types of storage:

- My Drive — personal storage for the user
- Company Drive — company storage
- Group Drive — storage for the working group

You can get a list of storage types using the [disk.storage.gettypes](./disk-storage-get-types.md) method.

{% note tip "User Documentation" %}

- [My Drive overview](https://helpdesk.bitrix24.com/open/7612115/)
- [Company Drive overview](https://helpdesk.bitrix24.com/open/19635400/)

{% endnote %}

## How to Get Storage Description

To work with storage, you need its identifier.

1. Retrieve a list of available storages using the [disk.storage.getlist](./disk-storage-get-list.md) method.
2. Find the required storage in the list and use its `ID`.
3. Get the storage parameters using the [disk.storage.get](./disk-storage-get.md) method.

The description of all storage fields is returned by the [disk.storage.getfields](./disk-storage-get-fields.md) method. To work with application storage, use the [disk.storage.getforapp](./disk-storage-get-for-app.md) method.

## Working with Storage Contents

In the root of the storage, you can perform the following operations:

- retrieve a list of files and folders using the [disk.storage.getchildren](./disk-storage-get-children.md) method
- create a folder using the [disk.storage.addfolder](./disk-storage-add-folder.md) method
- upload a file using the [disk.storage.uploadfile](./disk-storage-upload-file.md) method

To work with nested folders and files, use the [disk.folder.*](../folder/index.md) methods.

## How to Rename Storage

You can only rename application storage — for this, use the [disk.storage.rename](./disk-storage-rename.md) method. Personal, company and group storages cannot be renamed.

## Overview of Methods {#all-methods}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#|
|| **Method** | **Description** ||
|| [disk.storage.getfields](./disk-storage-get-fields.md) | Returns the description of storage fields ||
|| [disk.storage.get](./disk-storage-get.md) | Returns storage by identifier ||
|| [disk.storage.rename](./disk-storage-rename.md) | Renames application storage ||
|| [disk.storage.getlist](./disk-storage-get-list.md) | Returns a list of available storages ||
|| [disk.storage.gettypes](./disk-storage-get-types.md) | Returns a list of storage types ||
|| [disk.storage.addfolder](./disk-storage-add-folder.md) | Creates a folder in the root of the storage ||
|| [disk.storage.getchildren](./disk-storage-get-children.md) | Returns a list of files and folders located in the root of the storage ||
|| [disk.storage.uploadfile](./disk-storage-upload-file.md) | Uploads a new file to the root of the storage ||
|| [disk.storage.getforapp](./disk-storage-get-for-app.md) | Returns the description of application storage ||
|#