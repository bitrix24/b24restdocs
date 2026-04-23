# How to Work with Files

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Files can be transmitted to Bitrix24 fields in Base64 format or uploaded to Drive. The choice of method depends on the type of field and the method that accepts the file.

> Quick Navigation: [choose tutorial](#choose-tutorial)

## Types of File Fields

- **File.** The field is not linked to Drive. It accepts a Base64 string or an array containing the file name and Base64 string.
- **File (Drive).** The field is linked to Drive. It stores the `ID` of the Drive object. The methods [disk.folder.uploadfile](../disk/folder/disk-folder-upload-file.md), [disk.storage.uploadfile](../disk/storage/disk-storage-upload-file.md), and [disk.file.*](../disk/file/index.md) are suitable for uploading and updating such files.

## Connection with Other Objects

Files are associated with CRM entities, Drive, catalogs, chats, tasks, sites, and other Bitrix24 tools.

- **CRM.** Files can be transmitted to the fields of objects using the methods [crm.item.add](../crm/universal/crm-item-add.md) and [crm.item.update](../crm/universal/crm-item-update.md), as well as attached to comments using the methods [crm.timeline.comment.add](../crm/timeline/comments/crm-timeline-comment-add.md) and [crm.timeline.comment.update](../crm/timeline/comments/crm-timeline-comment-update.md).
- **Catalog.** Product images are transmitted to fields using the methods [catalog.product.add](../catalog/product/catalog-product-add.md) and [catalog.product.update](../catalog/product/catalog-product-update.md).
- **Lists.** Files in list item fields are added and updated using the methods [lists.element.add](../lists/elements/lists-element-add.md) and [lists.element.update](../lists/elements/lists-element-update.md).
- **Feed.** Files in feed messages are added and updated using the methods [log.blogpost.add](../log/log-blogpost-add.md) and [log.blogpost.update](../log/log-blogpost-update.md).
- **Documents.** Document templates are uploaded using the methods [documentgenerator.template.add](../document-generator/templates/document-generator-template-add.md) and [crm.documentgenerator.template.add](../crm/document-generator/templates/crm-document-generator-template-add.md).
- **Users.** A user's photo is transmitted using the method [user.add](../user/user-add.md).
- **Chats.** A file is uploaded to a chat using the method [im.v2.File.upload](../chat-bots/chat-bots-v2/im.v2/files/file-upload.md), and a download link is obtained using the method [im.v2.File.download](../chat-bots/chat-bots-v2/im.v2/files/file-download.md).
- **Chat-bots.** A file is uploaded on behalf of the bot using the method [imbot.v2.File.upload](../chat-bots/chat-bots-v2/imbot.v2/files/file-upload.md), and a download link is obtained using the method [imbot.v2.File.download](../chat-bots/chat-bots-v2/imbot.v2/files/file-download.md).
- **Tasks.** Files from Drive are attached to a task using the method [tasks.task.file.attach](../rest-v3/tasks/tasks-task-file-attach.md).
- **Sites.** An image for a site block is uploaded and linked using the method [landing.block.uploadfile](../landing/block/methods/landing-block-upload-file.md).

## Main Methods for Working with Files

#|
|| **Task** | **Methods** ||
|| Upload a file to Drive | [disk.folder.uploadfile](../disk/folder/disk-folder-upload-file.md), [disk.storage.uploadfile](../disk/storage/disk-storage-upload-file.md) ||
|| Update a file on Drive | [disk.file.uploadversion](../disk/file/disk-file-upload-version.md) ||
|| Get information about a file on Drive | [disk.file.get](../disk/file/disk-file-get.md) ||
|| Delete a file on Drive | [disk.file.delete](../disk/file/disk-file-delete.md) ||
|| Get a link to a file on Drive | [disk.file.getExternalLink](../disk/file/disk-file-get-external-link.md) ||
|#

## How to Get Started

1. Determine the type of field: file or file (Drive).
2. Encode the file in Base64 if you need to transmit the file to a regular field.
3. URL-encode the Base64 string if the file is transmitted via a GET request or cURL.
4. Upload the file to Drive and provide the `ID` of the Drive object if the field is linked to Drive.

## How to Choose a Tutorial {#choose-tutorial}

#|
|| **If you need** | **Open** ||
|| To transmit a new file to a Bitrix24 field or upload it to Drive | [How to Upload Files](./how-to-upload-files.md) ||
|| To replace a file, delete a file, or keep other files in a multiple field | [How to Update and Delete Files](./how-to-update-files.md) ||
|#