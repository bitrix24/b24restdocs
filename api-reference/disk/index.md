# Drive: Overview of Methods

Drive is a tool for working with files. It allows you to:

- store company documents and personal files
- upload, edit documents, and view change history
- configure access permissions for files and folders
- share files via public links with external users
- synchronize files with your computer

Drive is integrated with Tasks, Chats, CRM, Workgroups, and other Bitrix24 tools. This simplifies work by providing quick access to files, collaborative work on them, and automating document flow.

> Quick navigation: [all methods](#all-methods) 
>
> User documentation: [Bitrix24 Drive](https://helpdesk.bitrix24.com/open/20995470/)

## Drive Storage

There are three types of storage:

- personal user storage
- company shared storage
- workgroup storage

Storage can be managed using the group of methods [disk.storage.*](./storage/index.md).

{% note tip "User Documentation" %}

- [What is stored on My Drive in Bitrix24](https://helpdesk.bitrix24.com/open/7612115/)
- [Company Drive in Bitrix24](https://helpdesk.bitrix24.com/open/19635400/)

{% endnote %}

## Working with Folders and Files

Folders and files can be created, moved, deleted, and their access permissions can be modified. Use the groups of methods [disk.folder.*](./folder/index.md) and [disk.file.*](./file/index.md) for this.

You can find out the version of a file using the method [disk.version.get](./version/disk-version-get.md).

{% note tip "User Documentation" %}

- [Documents Online: Getting Started](https://helpdesk.bitrix24.com/open/20595366/)
- [Access Permissions for My Drive in Bitrix24](https://helpdesk.bitrix24.com/open/19900270/)
- [How long document versions are stored on Drive](https://helpdesk.bitrix24.com/open/18874394/)

{% endnote %}

## Drive Connection with Other Objects {#diskconnection}

**CRM.** Files can be attached to deals and estimates. The group of methods [crm.activity.*](../crm/timeline/activities/index.md) is responsible for working with activities, while [crm.quote.*](../crm/quote/crm-quote-add.md) handles estimates.

**Workflows.** You can initiate workflows for documents in the shared drive. Workflow management is performed using the methods [bizproc.workflow.*](../bizproc/index.md).

**Tasks.** Files are attached to task descriptions and comments. All task participants can view, edit, and download attached files. Work with tasks and comments should be done through the groups of methods [tasks.task.*](../tasks/index.md) and [task.commentitem.*](../tasks/comment-item/index.md).

**Calendar.** Files can be added to events and become accessible to all participants. You can create and modify events using the methods [calendar.event.*](../calendar/index.md).

**News Feed.** Files can be attached to messages and comments. The method [log.blogpost.add](../log/log-blogpost-add.md) creates a new message, while the method [log.blogcomment.add](../log/blogcomment/log-blogcomment-add.md) creates a new comment.

**Email.** Attachments from emails are saved on Drive. Emails can only be managed through the Bitrix24 interface. If an email is saved in CRM, attachments can be interacted with using the methods [crm.activity.*](../crm/timeline/activities/index.md) through the `FILES` field.

**Workgroups and Projects.** Drive is integrated [into workgroups and projects](../sonet-group/sonet-group-create.md), each having its own storage.

**Universal Lists.** List items are linked to Drive through the [field](../lists/fields/index.md) of type File (Drive). You can create and modify items using the methods [lists.element.*](../lists/elements/index.md).

**Chats.** Users can exchange documents, photos, videos, and audio. Files can be viewed and downloaded, and documents can be edited in the chat without downloading. Attached files are available to all chat participants. The method [im.message.add](../chats/messages/im-message-add.md) sends a message with a file in the chat.

{% note tip "User Documentation" %}

- [Workflows on Drive in Bitrix24](https://helpdesk.bitrix24.com/open/20989316/)
- [Drive in Groups and Projects](https://helpdesk.bitrix24.com/open/22074250/)
- [How to Upload a File from Cloud Storage to Bitrix24](https://helpdesk.bitrix24.com/open/20080420/)
- [How to Use Public and Internal Links to Files in Bitrix24](https://helpdesk.bitrix24.com/open/20080420/)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`disk`](../scopes/permissions.md)
>
> Who can execute the method: any user

### Version

#|
|| **Method** | **Description** ||
|| [disk.version.get](version/disk-version-get.md) | Returns the version by identifier ||
|#

### Folder

#|
|| **Method** | **Description** ||
|| [disk.folder.getfields](folder/disk-folder-get-fields.md) | Returns the description of folder fields ||
|| [disk.folder.get](folder/disk-folder-get.md) | Returns the folder by identifier ||
|| [disk.folder.getchildren](folder/disk-folder-get-children.md) | Returns a list of files and folders located in the folder ||
|| [disk.folder.addsubfolder](folder/disk-folder-add-subfolder.md) | Creates a subfolder ||
|| [disk.folder.copyto](folder/disk-folder-copy-to.md) | Copies the folder to the specified folder ||
|| [disk.folder.moveto](folder/disk-folder-move-to.md) | Moves the folder to the specified folder ||
|| [disk.folder.rename](folder/disk-folder-rename.md) | Renames the folder ||
|| [disk.folder.deletetree](folder/disk-folder-delete-tree.md) | Permanently deletes the folder and all its contents ||
|| [disk.folder.markdeleted](folder/disk-folder-mark-deleted.md) | Moves the folder to the trash ||
|| [disk.folder.restore](folder/disk-folder-restore.md) | Restores the folder from the trash ||
|| [disk.folder.uploadfile](folder/disk-folder-upload-file.md) | Uploads a new file to the specified folder ||
|| [disk.folder.getexternallink](folder/disk-folder-get-external-link.md) | Returns a public link to the folder ||
|#

### Access Permissions

#|
|| **Method** | **Description** ||
|| [disk.rights.getTasks](rights/disk-rights-get-tasks.md) | Allows you to get a list of access levels that can be used in assigning permissions ||
|#

### Attached File

#|
|| **Method** | **Description** ||
|| [disk.attachedObject.get](attached-object/disk-attached-object-get.md) | Returns information about the attached file ||
|#

### File

#|
|| **Method** | **Description** ||
|| [disk.file.getfields](file/disk-file-get-fields.md) | Returns the description of file fields ||
|| [disk.file.get](file/disk-file-get.md) | Returns the file by identifier ||
|| [disk.file.rename](file/disk-file-rename.md) | Renames the file ||
|| [disk.file.copyto](file/disk-file-copy-to.md) | Copies the file to the specified folder ||
|| [disk.file.moveto](file/disk-file-move-to.md) | Moves the file to the specified folder ||
|| [disk.file.delete](file/disk-file-delete.md) | Permanently deletes the file ||
|| [disk.file.markdeleted](file/disk-file-mark-deleted.md) | Moves the file to the trash ||
|| [disk.file.restore](file/disk-file-restore.md) | Restores the file from the trash ||
|| [disk.file.uploadversion](file/disk-file-upload-version.md) | Uploads a new version of the file ||
|| [disk.file.getVersions](file/disk-file-get-versions.md) | Returns a list of file versions ||
|| [disk.file.restoreFromVersion](file/disk-file-restore-from-version.md) | Restores the file from a specific version ||
|| [disk.file.getExternalLink](file/disk-file-get-external-link.md) | Returns a public link to the file ||
|#

### Storage

#|
|| **Method** | **Description** ||
|| [disk.storage.getfields](storage/disk-storage-get-fields.md) | Returns the description of storage fields ||
|| [disk.storage.get](storage/disk-storage-get.md) | Returns the storage by identifier ||
|| [disk.storage.rename](storage/disk-storage-rename.md) | Renames the storage. Renaming is only allowed for application storage (see [disk.storage.getforapp](storage/disk-storage-get-for-app.md)) ||
|| [disk.storage.getlist](storage/disk-storage-get-list.md) | Returns a list of available storages ||
|| [disk.storage.gettypes](storage/disk-storage-get-types.md) | Returns a list of storage types ||
|| [disk.storage.addfolder](storage/disk-storage-add-folder.md) | Creates a folder in the root of the storage ||
|| [disk.storage.getchildren](storage/disk-storage-get-children.md) | Returns a list of files and folders that are directly in the root of the storage ||
|| [disk.storage.uploadfile](storage/disk-storage-upload-file.md) | Uploads a new file to the root of the storage ||
|| [disk.storage.getforapp](storage/disk-storage-get-for-app.md) | Returns the description of the storage that the application can work with to store its data (files and folders) ||
|#