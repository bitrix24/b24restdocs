# REST Methods Available for Working with Disk

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

> Scope: [`disk`](../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| **Version** | ||
|| [disk.version.get](version/disk-version-get.md) | Returns the version by identifier. ||
|| **Folder** | ||
|| [disk.folder.getfields](folder/disk-folder-get-fields.md) | Returns the description of folder fields. ||
|| [disk.folder.get](folder/disk-folder-get.md) | Returns the folder by identifier. ||
|| [disk.folder.getchildren](folder/disk-folder-get-children.md) | Returns a list of files and folders that are directly in the folder. ||
|| [disk.folder.addsubfolder](folder/disk-folder-add-subfolder.md) | Creates a subfolder. ||
|| [disk.folder.copyto](folder/disk-folder-copy-to.md) | Copies the folder to the specified folder. ||
|| [disk.folder.moveto](folder/disk-folder-move-to.md) | Moves the folder to the specified folder. ||
|| [disk.folder.rename](folder/disk-folder-rename.md) | Renames the folder. ||
|| [disk.folder.deletetree](folder/disk-folder-delete-tree.md) | Permanently deletes the folder and all its child elements. ||
|| [disk.folder.markdeleted](folder/disk-folder-mark-deleted.md) | Moves the folder to the trash. ||
|| [disk.folder.restore](folder/disk-folder-restore.md) | Restores the folder from the trash. ||
|| [disk.folder.uploadfile](folder/disk-folder-upload-file.md) | Uploads a new file to the specified folder. ||
|| [disk.folder.getExternalLink](folder/disk-folder-get-external-link.md) | This method returns a public link. ||
|| **Attached File** | ||
|| [disk.attachedObject.get](attached-object/disk-attached-object-get.md) | Returns information about the attached file. ||
|| **File** | ||
|| [disk.file.getfields](file/disk-file-get-fields.md) | Returns the description of file fields. ||
|| [disk.file.get](file/disk-file-get.md) | Returns the file by identifier. ||
|| [disk.file.rename](file/disk-file-rename.md) | Renames the file. ||
|| [disk.file.copyto](file/disk-file-copy-to.md) | Copies the file to the specified folder. ||
|| [disk.file.moveto](file/disk-file-move-to.md) | Moves the file to the specified folder. ||
|| [disk.file.delete](file/disk-file-delete.md) | Permanently deletes the file. ||
|| [disk.file.markdeleted](file/disk-file-mark-deleted.md) | Moves the file to the trash. ||
|| [disk.file.restore](file/disk-file-restore.md) | Restores the file from the trash. ||
|| [disk.file.uploadversion](file/disk-file-upload-version.md) | Uploads a new version of the file. ||
|| [disk.file.getVersions](file/disk-file-get-versions.md) | Returns a list of file versions. ||
|| [disk.file.restoreFromVersion](file/disk-file-restore-from-version.md) | Restores the file from a specific version. ||
|| [disk.file.getExternalLink](file/disk-file-get-external-link.md) | Returns a public link to the file. ||
|| **Storage** | ||
|| [disk.storage.getfields](storage/disk-storage-get-fields.md) | Returns the description of storage fields. ||
|| [disk.storage.get](storage/disk-storage-get.md) | Returns the storage by identifier. ||
|| [disk.storage.rename](storage/disk-storage-rename.md) | Renames the storage. Renaming is only allowed for application storage (see [disk.storage.getforapp](storage/disk-storage-get-for-app.md)). ||
|| [disk.storage.getlist](storage/disk-storage-get-list.md) | Returns a list of available storages. ||
|| [disk.storage.gettypes](storage/disk-storage-get-types.md) | Returns a list of storage types. ||
|| [disk.storage.addfolder](storage/disk-storage-add-folder.md) | Creates a folder in the root of the storage. ||
|| [disk.storage.getchildren](storage/disk-storage-get-children.md) | Returns a list of files and folders that are directly in the root of the storage. ||
|| [disk.storage.uploadfile](storage/disk-storage-upload-file.md) | Uploads a new file to the root of the storage. ||
|| [disk.storage.getforapp](storage/disk-storage-get-for-app.md) | Returns the description of the storage that the application can use to store its data (files and folders). ||
|| **Access Rights** | ||
|| [disk.rights.getTasks](rights/disk-rights-get-tasks.md) | This method allows you to get a list of access levels that can be used in assigning permissions. ||
|#