# Drive Storage: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Storage is the Drive in Bitrix24 where you can store documents and files, create folders, and retrieve lists of contents.

> Quick navigation: [all methods](#all-methods)

## Types of Storage

The [disk.storage.getTypes](./disk-storage-get-types.md) method returns three main storage types:

- My Drive — personal storage for the user
- Company Drive — company storage
- Group Drive — storage for the working group

Application storage uses the special `restapp` type. Retrieve or create this storage using [disk.storage.getForApp](./disk-storage-get-for-app.md). The `disk.storage.getTypes` result does not include `restapp`.

{% note tip "User Documentation" %}

- [My Drive overview](https://helpdesk.bitrix24.com/open/7612115/)
- [Company Drive overview](https://helpdesk.bitrix24.com/open/19635400/)

{% endnote %}

## How to Start

To work with storage, you need its identifier.

1. Retrieve a list of available storages using [disk.storage.getList](./disk-storage-get-list.md). For application storage, use [disk.storage.getForApp](./disk-storage-get-for-app.md)
2. Select the required storage and save its `ID`
3. Get the storage parameters using the [disk.storage.get](./disk-storage-get.md) method
4. Retrieve files and folders in the root using [disk.storage.getChildren](./disk-storage-get-children.md)

In the `disk.storage.get` response, the `ROOT_OBJECT_ID` field contains the root folder identifier. The `ID` and `ROOT_OBJECT_ID` fields are returned as strings. Retrieve descriptions of all storage fields using [disk.storage.getFields](./disk-storage-get-fields.md).

## Relationship with Other Objects

A storage is the entry point for working with folders, files, and application data.

**Folders.** The `ROOT_OBJECT_ID` field contains the root folder identifier. [disk.storage.getChildren](./disk-storage-get-children.md) returns its contents, while [disk.storage.addFolder](./disk-storage-add-folder.md) creates a folder in it. For nested folders, use the [disk.folder.*](../folder/index.md) methods.

**Files.** [disk.storage.uploadFile](./disk-storage-upload-file.md) uploads a file to the storage root. To continue working with the uploaded file, use its `ID` in the [disk.file.*](../file/index.md) methods.

**Application.** [disk.storage.getForApp](./disk-storage-get-for-app.md) returns the current application's storage. Only this storage can be renamed using [disk.storage.rename](./disk-storage-rename.md).

## Errors When Working with Storages

Methods that accept a storage identifier return `ERROR_NOT_FOUND` if the storage is not found. If permissions are insufficient, read and modification methods return `ACCESS_DENIED`.

The `disk.storage.getForApp` method returns `ACCESS_DENIED` outside the application context.

## Overview of Methods {#all-methods}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the methods: depends on the method

#|
|| **Method** | **Description** ||
|| [disk.storage.getFields](./disk-storage-get-fields.md) | Returns the description of storage fields ||
|| [disk.storage.get](./disk-storage-get.md) | Returns storage by identifier ||
|| [disk.storage.rename](./disk-storage-rename.md) | Renames application storage ||
|| [disk.storage.getList](./disk-storage-get-list.md) | Returns a list of available storages ||
|| [disk.storage.getTypes](./disk-storage-get-types.md) | Returns a list of storage types ||
|| [disk.storage.addFolder](./disk-storage-add-folder.md) | Creates a folder in the root of the storage ||
|| [disk.storage.getChildren](./disk-storage-get-children.md) | Returns a list of files and folders located in the root of the storage ||
|| [disk.storage.uploadFile](./disk-storage-upload-file.md) | Uploads a new file to the root of the storage ||
|| [disk.storage.getForApp](./disk-storage-get-for-app.md) | Returns the description of application storage ||
|#
