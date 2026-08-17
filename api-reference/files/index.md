# How to Work with Files

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A file reaches Bitrix24 in one of two main ways: it is passed as a Base64 string directly into a method field, or it is uploaded to Drive and the `ID` of the Drive object is passed into the field. For large files, Drive offers a third way — an upload to a separate `UploadUrl` address. The way depends on the field type and on the method that accepts the file.

The choice is determined by the field type, the shape of the method response, the permissions, and the request limitations. The format of a specific request, as well as updating and deleting files, are covered in separate articles.

> Quick Navigation: [choose tutorial](#choose-tutorial)

## Types of File Fields

The field type determines the request format and the way the file is updated. The field type is specified in the parameter description on the method's page.

- **File.** The field is not linked to Drive. The file is passed directly into the field — either as a Base64 string or as an array containing the file name and such a string, for example `["report.pdf", "JVBERi0xLjQKJeLj"]`. Bitrix24 decodes the string, saves the file, and stores the `ID` of the file in the field.

- **File (Drive).** The field is linked to Drive, and the field stores the `ID` of an object on Drive. Such fields exist in lists, tasks, and the feed. A method usually expects a ready-made `ID`: first upload the file using the [disk.folder.uploadFile](../disk/folder/disk-folder-upload-file.md) or [disk.storage.uploadFile](../disk/storage/disk-storage-upload-file.md) method, then pass the `ID` from the response into the object field. Some methods also accept Base64 and save the file to Drive themselves. Both ways are covered in the [How to Pass a File to a Field Linked to Drive](./how-to-upload-files.md#disk-field) section.

There are no "file (Drive)" type fields in the CRM: the file fields of CRM items are of the "file" type. The file does not become a Drive object, and the links to it arrive in the response of the method that reads the item. Timeline comment attachments are an exception at the method level rather than a field type: parameter `FILES` accepts Base64, and the file is saved to Drive.

In the CRM, the format depends on the method: [crm.item.add](../crm/universal/crm-item-add.md) and [crm.item.update](../crm/universal/crm-item-update.md) accept a "name — Base64" array in a field such as `ufCrm_7_1739432938`, while [crm.lead.add](../crm/leads/crm-lead-add.md), [crm.deal.add](../crm/deals/crm-deal-add.md), [crm.contact.add](../crm/contacts/crm-contact-add.md), and [crm.company.add](../crm/companies/crm-company-add.md) accept a `fileData` object in a field such as `UF_CRM_1711610801`. A minimal [crm.item.add](../crm/universal/crm-item-add.md) request looks like this.

```json
{
    "entityTypeId": 177,
    "fields": {
        "title": "Contract",
        "ufCrm_7_1739432938": ["report.pdf", "JVBERi0xLjQKJeLj"]
    }
}
```

## Relationships with Other Objects {#objects}

Files are not stored on their own — they are stored in the fields of Bitrix24 objects. The link works through an object field or through a separate method parameter. Every method has its own transfer format, so check the [How to Choose a Transfer Format](./how-to-upload-files.md#formats) table.

#|
|| **Object** | **Where the file is passed** | **Methods** ||
|| CRM object | Custom field of the "file" type | [crm.item.add](../crm/universal/crm-item-add.md), [crm.item.update](../crm/universal/crm-item-update.md) ||
|| CRM comment | Field `FILES` | [crm.timeline.comment.add](../crm/timeline/comments/crm-timeline-comment-add.md), [crm.timeline.comment.update](../crm/timeline/comments/crm-timeline-comment-update.md) ||
|| Catalog | Fields `previewPicture` and `detailPicture`, or a product property; a property value has its own `valueId`, which is required for updating and deleting | [catalog.product.add](../catalog/product/catalog-product-add.md), [catalog.product.update](../catalog/product/catalog-product-update.md) ||
|| Additional product images | Parameter `fileContent` | [catalog.productImage.add](../catalog/product-image/catalog-product-image-add.md) ||
|| Lists | Item property such as `PROPERTY_1075` | [lists.element.add](../lists/elements/lists-element-add.md), [lists.element.update](../lists/elements/lists-element-update.md) ||
|| Data storage | Property of the "file" type | [entity.item.add](../entity/items/entity-item-add.md), [entity.item.update](../entity/items/entity-item-update.md) ||
|| Feed | Field `FILES`, and `UF_BLOG_POST_FILE` for posts | [log.blogpost.add](../log/log-blogpost-add.md), [log.blogpost.update](../log/log-blogpost-update.md), [log.blogcomment.add](../log/blogcomment/log-blogcomment-add.md) ||
|| Knowledge base | Parameters `fileName` and `fileContent` | [note.file.add](../note/file/note-file-add.md) ||
|| Document templates | Field `file`; for [documentgenerator.template.add](../document-generator/templates/document-generator-template-add.md), you can pass the `ID` of a file already stored on Drive in `fileId` instead | [documentgenerator.template.add](../document-generator/templates/document-generator-template-add.md), [crm.documentgenerator.template.add](../crm/document-generator/templates/crm-document-generator-template-add.md) ||
|| Workflow templates | Field `TEMPLATE_DATA` | [bizproc.workflow.template.add](../bizproc/template/bizproc-workflow-template-add.md) ||
|| Users | Field `PERSONAL_PHOTO` | [user.add](../user/user-add.md), [user.update](../user/user-update.md) ||
|| Chats | Fields `name` and `content` in the `fields` object | [im.v2.File.upload](../chat-bots/chat-bots-v2/im.v2/files/file-upload.md) — on behalf of a user, [imbot.v2.File.upload](../chat-bots/chat-bots-v2/imbot.v2/files/file-upload.md) — on behalf of a bot ||
|| Tasks | Parameter `fileIds` or field `UF_TASK_WEBDAV_FILES` | [tasks.task.file.attach](../tasks/tasks-task-file-attach.md), [tasks.task.add](../tasks/tasks-task-add.md) ||
|| Sites | Parameter `picture`, only an image is accepted | [landing.block.uploadfile](../landing/block/methods/landing-block-upload-file.md) ||
|| Telephony | Parameters `FILENAME` and `FILE_CONTENT` | [telephony.externalCall.attachRecord](../telephony/telephony-external-call-attach-record.md) ||
|#

What to keep in mind about specific tools.

- **The name of a file field** is returned by the [crm.item.fields](../crm/universal/crm-item-fields.md) and [lists.field.get](../lists/fields/lists-field-get.md) methods. For a product, [catalog.productProperty.list](../catalog/product-property/catalog-product-property-list.md) returns the property identifier, and the field name is composed as `property{id}`. The `valueId` of a product property is returned by [catalog.product.get](../catalog/product/catalog-product-get.md).

- **In the CRM, choose the universal methods.** [crm.item.add](../crm/universal/crm-item-add.md) and [crm.item.update](../crm/universal/crm-item-update.md) work with all CRM objects, including Smart Processes. Updating file fields with the paired methods `crm.lead.update`, `crm.deal.update`, `crm.contact.update`, and `crm.company.update` is not recommended — for details, see the [Update a Field in a CRM Object](./how-to-update-files.md#crm-item-update) section.

- **A file that is already on Drive is linked by its `ID` with the `n` prefix** — in the format `["n12345"]`. This is how field `UF_TASK_WEBDAV_FILES` in [tasks.task.add](../tasks/tasks-task-add.md) and field `UF_BLOG_POST_FILE` in [log.blogpost.add](../log/log-blogpost-add.md) work. If field `FILES` is passed in the same request, the value of `UF_BLOG_POST_FILE` is ignored.

- **In the Knowledge base, a file is not inserted into the text automatically.** [note.file.add](../note/file/note-file-add.md) only saves the file and links it to a document: take the `assetMarkdown` block from the response and add it to the document content. Metadata and a ready-made block by identifier are returned by [note.file.get](../note/file/note-file-get.md). These methods belong to [REST 3.0](../rest-v3.md): `/api/` is added to the call address.

## What Arrives in the Response

What arrives in `result` depends on the method. [crm.item.add](../crm/universal/crm-item-add.md) and [crm.item.update](../crm/universal/crm-item-update.md) return the entire `item` object together with the file field — a separate read request is not needed. The [crm.lead.add](../crm/leads/crm-lead-add.md), [lists.element.add](../lists/elements/lists-element-add.md), [log.blogpost.add](../log/log-blogpost-add.md), and [user.add](../user/user-add.md) methods return only the identifier of the created object, so the files have to be requested with a read method. The exact schema is in the "Returned Data" section on the method's page.

The shape of a file field in the response differs from tool to tool.

#|
|| **Object** | **What arrives in the file field** | **How to retrieve a link to the file** ||
|| CRM item | An object with `id`, `url`, and `urlMachine`; for a multiple field, an array of such objects | The links arrive directly in the [crm.item.get](../crm/universal/crm-item-get.md) response ||
|| List item | An object where the key is the `ID` of the property value and the value is the `ID` of the file | [lists.element.get.file.url](../lists/elements/lists-element-get-file-url.md) ||
|| Feed post | An array of file `ID` | There is no link in the response: the post files are stored on the author's Drive ||
|| Product | In a property — an array of objects where `value` contains `id`, `url`, and `urlMachine`, and `valueId` is the identifier of the value; in fields `previewPicture` and `detailPicture` — a single object with `id`, `url`, and `urlMachine` | The links arrive directly in the [catalog.product.get](../catalog/product/catalog-product-get.md) response ||
|| File on Drive | A file object: `ID`, name, size, `DOWNLOAD_URL` | The link arrives directly in the [disk.file.get](../disk/file/disk-file-get.md) response ||
|#

The table covers the common cases; for other tools, check the shape of the file field on the method's page. A link to a file in a chat is returned by dedicated methods — [im.v2.File.download](../chat-bots/chat-bots-v2/im.v2/files/file-download.md) and [imbot.v2.File.download](../chat-bots/chat-bots-v2/imbot.v2/files/file-download.md).

Sample responses and the rules for downloading via a signed link are in the [Response Content](./how-to-upload-files.md#response-content) section. The `urlMachine` and `DOWNLOAD_URL` links contain an authorization token: do not publish them and do not record them in logs.

If a file does not appear in the field, the method usually returns no error — the field stays empty or loses its previous files. How a method handles old files is shown in the [How Methods Handle Files](./how-to-update-files.md#behavior) table, and the general shape of an error response is described in [Error Codes](../../error-codes.md).

## Permissions and Limitations

The User permissions and the [scope](../scopes/permissions.md) are specified at the beginning of each method's page. Permissions for a file in an object field are inherited from the object: whoever can modify the item can also modify its files. Drive objects have their own permissions that are not inherited from the object whose field holds the file: they are checked against a specific folder, storage, or file.

The limitations are common to all methods that accept a file in the request body.

- A file is passed via a POST request: a Base64 string is almost always longer than the address bar limit, which is about 2048 characters in browsers and web servers.

- A Base64 string is approximately one-third longer than the original file — refer to the size of the string.

- The POST request size in the Cloud is limited to 2 GB; in the Self-hosted version, it is limited by your server settings.

- The request execution time in the Cloud is limited to 60 seconds.

A file that does not fit within the request limit is uploaded to Drive in two steps. Call [disk.folder.uploadFile](../disk/folder/disk-folder-upload-file.md) without the `fileContent` parameter — the method returns an `UploadUrl` address and the `field` form field name. The file is then sent to that address in a separate request, as described in the [Uploading a File via URL](../disk/folder/disk-folder-upload-file.md#uploadurl) section.

Uploading a call recording works the same way: [telephony.externalCall.attachRecord](../telephony/telephony-external-call-attach-record.md) with the `FILENAME` parameter and without `FILE_CONTENT` returns `uploadUrl` and `fieldName`. Other methods have no such workaround — the file has to fit within the request limit.

Some methods have stricter limits: [im.v2.File.upload](../chat-bots/chat-bots-v2/im.v2/files/file-upload.md) accepts a file of up to 100 MB and returns the `FILE_TOO_LARGE` error, while [note.file.add](../note/file/note-file-add.md) is limited by the `main.max_file_size` setting or, if it is not set, by 25 MiB. The full list of limitations with figures is in the [Limitations When Working with Files](./how-to-upload-files.md#limitations-when-working-with-files) section.

## How to Get Started

1. Open the method's page and check what it expects in the parameter description: the file content — a string or an array with Base64 — or a ready-made `ID` of an object on Drive.

2. Check the [How to Choose a Transfer Format](./how-to-upload-files.md#formats) table: a string, a "name — Base64" array, a `fileData` object, or a separate parameter.

3. Encode the file in [Base64](./how-to-upload-files.md#how-to-encode-a-file-in-base64) if the method accepts the file content.

4. Upload the file to Drive and pass the `ID` of the object if the method expects a ready-made `ID`. The list of available storages is returned by the [disk.storage.getList](../disk/storage/disk-storage-get-list.md) method, and the application's own storage is returned by [disk.storage.getForApp](../disk/storage/disk-storage-get-for-app.md), which an administrator calls in the application context. The `ID` of a folder inside a storage is returned by the [disk.storage.getChildren](../disk/storage/disk-storage-get-children.md) and [disk.folder.getChildren](../disk/folder/disk-folder-get-children.md) methods.

5. Request the object with a read method to retrieve the `ID` of the files and the download links. These `ID` will be required when the files need to be updated or deleted.

## Drive Methods for Working with Files

A file on Drive is a standalone object, and it is handled by Drive methods rather than by the methods of the object whose field holds it. Below are the ones needed when uploading, updating, and deleting a file. The methods that accept a file in a field of their own object are listed in the [Relationships with Other Objects](#objects) section, and the entire Drive section is covered in the [Drive](../disk/index.md) article.

#|
|| **Method** | **Description** ||
|| [disk.folder.uploadFile](../disk/folder/disk-folder-upload-file.md) | Uploads a file to the specified folder ||
|| [disk.storage.uploadFile](../disk/storage/disk-storage-upload-file.md) | Uploads a file to the root of a storage ||
|| [disk.file.uploadVersion](../disk/file/disk-file-upload-version.md) | Uploads a new version of a file ||
|| [disk.file.get](../disk/file/disk-file-get.md) | Returns the parameters of a file ||
|| [disk.file.getExternalLink](../disk/file/disk-file-get-external-link.md) | Returns a public link to a file: anyone who has it can open it, unlike the token-signed `urlMachine` and `DOWNLOAD_URL` ||
|| [disk.file.markDeleted](../disk/file/disk-file-mark-deleted.md) | Moves a file to the recycle bin, from which it can be restored ||
|| [disk.file.delete](../disk/file/disk-file-delete.md) | Deletes a file permanently, bypassing the recycle bin ||
|#

## How to Choose a Tutorial {#choose-tutorial}

#|
|| **If you need** | **Open** ||
|| To pass a new file to a Bitrix24 field or upload it to Drive | [How to Upload Files](./how-to-upload-files.md) ||
|| To replace a file, delete a file, or retain the other files in a multiple field | [How to Update and Delete Files](./how-to-update-files.md) ||
|| To pass a file in a GET request or via cURL | [Data Encoding](../../settings/how-to-call-rest-api/data-encoding.md) — the Base64 string has to be URL-encoded ||
|| To download a file by the link from the response | [Response Content](./how-to-upload-files.md#response-content) ||
|| To troubleshoot a method error | [Error Codes](../../error-codes.md) ||
|#
