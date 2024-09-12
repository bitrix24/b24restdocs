# Methods for Working with Storage

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [disk.storage.getfields](./disk-storage-get-fields.md) | Returns the description of the storage fields. ||
|| [disk.storage.get](./disk-storage-get.md) | Retrieves the storage by its identifier. ||
|| [disk.storage.rename](./disk-storage-rename.md) | Renames the storage. Only the application storage can be renamed (see [disk.storage.getforapp](./disk-storage-get-for-app.md)). ||
|| [disk.storage.getlist](./disk-storage-get-list.md) | Returns a list of available storages. ||
|| [disk.storage.gettypes](./disk-storage-get-types.md) | Returns a list of storage types. ||
|| [disk.storage.addfolder](./disk-storage-add-folder.md) | Creates a folder in the root of the storage. ||
|| [disk.storage.getchildren](./disk-storage-get-children.md) | Returns a list of files and folders that are directly in the root of the storage. ||
|| [disk.storage.uploadfile](./disk-storage-upload-file.md) | Uploads a new file to the root of the storage. ||
|| [disk.storage.getforapp](./disk-storage-get-for-app.md) | Returns the description of the storage that the application can work with to store its data (files and folders). ||
|#