# How to Download Files

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Files are downloaded by a link from a method response or by a method that returns the file directly. If a method returned a download link, [make a separate `GET` request](#request) to that link: the REST method call only retrieves the link, and the file itself is not embedded in the JSON response.

The REST API does not return the contents of a file field in Base64: Base64 is used to upload a file to Bitrix24, while downloading returns a URL or a file response.

## Types of File Fields

To download a file, first determine where it is stored.

- **File.** The field is not linked to Drive. The field stores the file `ID`, and the method that reads the object returns a link for opening or downloading the file. This `ID` cannot be passed to [disk.file.get](../disk/file/disk-file-get.md)

- **File (Drive).** The field is linked to Drive. The field stores the `ID` of a Drive object or the identifier of the file attachment to an object. The link is returned by Drive methods or by methods of the object to which the file is attached

{% note warning "" %}

Links for applications contain an authorization token. Do not publish them, do not pass them to client-side code unless necessary, and do not write them to logs.

{% endnote %}

## How to Choose a File Retrieval Method {#methods}

#|
|| **Where the File Is Located** | **How to Retrieve Download Data** | **Field or Result** ||
|| CRM custom field of the `file` type | [crm.item.get](../crm/universal/crm-item-get.md), [crm.item.list](../crm/universal/crm-item-list.md) | `urlMachine` ||
|| CRM timeline comment | [crm.timeline.comment.get](../crm/timeline/comments/crm-timeline-comment-get.md), [crm.timeline.comment.list](../crm/timeline/comments/crm-timeline-comment-list.md) | File `id` in `FILES`, then `DOWNLOAD_URL` from [disk.file.get](../disk/file/disk-file-get.md) ||
|| Feed post or comment | [log.blogpost.get](../log/log-blogpost-get.md), [log.blogcomment.user.get](../log/blogcomment/log-blogcomment-user-get.md) | `FILES`, then `DOWNLOAD_URL` from [disk.file.get](../disk/file/disk-file-get.md) ||
|| File on Drive | [disk.file.get](../disk/file/disk-file-get.md) | `DOWNLOAD_URL` ||
|| Attached Drive file, for example in a task or list | [disk.attachedObject.get](../disk/attached-object/disk-attached-object-get.md) | `OBJECT_ID`, then `DOWNLOAD_URL` from [disk.file.get](../disk/file/disk-file-get.md) ||
|| Task files | [tasks.task.get](../tasks/tasks-task-get.md), [tasks.task.get REST v3](../tasks/tasks-task-get-rest-v3.md) | `UF_TASK_WEBDAV_FILES` for individual files, `archiveLink` for an archive ||
|| List item | [lists.element.get.file.url](../lists/elements/lists-element-get-file-url.md) | URL from the `result` array ||
|| Data storage item | [entity.item.get](../entity/items/entity-item-get.md) | File field value; the field name depends on the storage configuration ||
|| User photo | [user.get](../user/user-get.md) | URL in the `PERSONAL_PHOTO` field ||
|| Catalog product | [catalog.product.get](../catalog/product/catalog-product-get.md), [catalog.product.list](../catalog/product/catalog-product-list.md) | `urlMachine` from the product field or the [catalog.product.download](../catalog/product/catalog-product-download.md) method ||
|| Telephony call recording | [voximplant.statistic.get](../telephony/voximplant/voximplant-statistic-get.md) | `CALL_RECORD_URL` ||
|| Document generator template | [documentgenerator.template.get](../document-generator/templates/document-generator-template-get.md), [documentgenerator.template.list](../document-generator/templates/document-generator-template-list.md), [crm.documentgenerator.template.get](../crm/document-generator/templates/crm-document-generator-template-get.md), [crm.documentgenerator.template.list](../crm/document-generator/templates/crm-document-generator-template-list.md) | `downloadMachine` ||
|| Document generator document | [documentgenerator.document.add](../document-generator/document-generator-document-add.md), [documentgenerator.document.list](../document-generator/document-generator-document-list.md), [crm.documentgenerator.document.add](../crm/document-generator/documents/crm-document-generator-document-add.md), [crm.documentgenerator.document.list](../crm/document-generator/documents/crm-document-generator-document-list.md) | `downloadUrlMachine` ||
|| Signed document | [sign.b2e.hcmlink.document.get](../sign/sign-b2e-hcmlink-document-get.md), [sign.b2e.mysafe.tail](../sign/sign-b2e-mysafe-tail.md), [sign.b2e.personal.tail](../sign/sign-b2e-personal-tail.md) | `fileUrl`, `file_url` ||
|| Knowledge base file | [note.file.get](../note/file/note-file-get.md) | File metadata and `assetMarkdown` for inserting the file into a document. The method does not return a download link or the file body ||
|| Chat file on behalf of a user | [im.v2.File.download](../chat-bots/chat-bots-v2/im.v2/files/file-download.md) | `downloadUrl` ||
|| Chat file on behalf of a bot | [imbot.v2.File.download](../chat-bots/chat-bots-v2/imbot.v2/files/file-download.md) | `downloadUrl` ||
|#

## Types of Links in Responses {#links}

Method responses contain links for users and links for applications.

#|
|| **Field** | **Meaning** | **When to Use It** ||
|| `url`, `urlShow`, `DETAIL_URL` | Link for opening the file in the Bitrix24 interface or in a browser with an authorized user | When a user opens the file in Bitrix24 ||
|| `urlMachine`, `DOWNLOAD_URL`, `downloadMachine`, `downloadUrlMachine`, `downloadUrl`, `fileUrl`, `file_url`, `CALL_RECORD_URL`, `archiveLink` | Download link. Often contains a token and lets a file be retrieved with a separate HTTP request | When an integration or server application downloads the file. Check the limitations on the method page: for example, in chats, `downloadUrl` is single-use ||
|| `urlDownload` | Download link in an authorized Bitrix24 context. In CRM timeline comments, it does not contain a REST token | When a user or an application opens the file in the Bitrix24 interface. To download a Drive file on the server side, retrieve `DOWNLOAD_URL` with [disk.file.get](../disk/file/disk-file-get.md) ||
|| URL without a separate field name, for example a URL from the `result` array or the `PERSONAL_PHOTO` value | The link arrives as a string in the method response | When the method returns a ready-made path to the file without an object containing `url` and `urlMachine` fields ||
|#

Not all methods in the table above return links. The [catalog.product.download](../catalog/product/catalog-product-download.md) method returns the file body directly, and [note.file.get](../note/file/note-file-get.md) returns metadata and `assetMarkdown`.

Links can be absolute or relative. For example, a task `archiveLink` or a catalog product `urlMachine` can be relative. If a link starts with `/`, add the Bitrix24 address to it:

```text
https://your-domain.bitrix24.com/bitrix/tools/disk/uf.php?attachedId=10&action=download&ncc=1
```

A link can be single-use or time-limited. If the HTTP response indicates an expired link or access denial, retrieve the link again with the object read method and download the file using the new link.

## Permissions and Limitations

- To download a file, you need permissions for the object from which the link was retrieved and the scope of the method that retrieves the link or file. For example, a file in a CRM field requires read permission for the CRM item and the `crm` scope, a Drive file requires permissions for the file or folder and the `disk` scope, and a chat file requires access to the chat and the `im` or `imbot` scope. The exact scope is specified on each method page and in the [Application Scope Permissions](../scopes/permissions.md) article

- A link for an application is not a permanent file identifier. Store the file `ID`, attachment identifier, or object `ID`, and retrieve the link before downloading

## Download a File from a CRM Field {#crm}

For CRM file fields, use the universal methods [crm.item.get](../crm/universal/crm-item-get.md) and [crm.item.list](../crm/universal/crm-item-list.md). They work with leads, deals, contacts, companies, invoices, and Smart Processes.

In the response, the file field contains `id`, `url`, and `urlMachine`. To download the file from an application, use `urlMachine`.

```json
{
    "result": {
        "item": {
            "id": 1,
            "ufCrm_123456": [
                {
                    "id": 10,
                    "url": "https://your-domain.bitrix24.com/bitrix/services/main/ajax.php?action=crm.controller.item.getFile&SITE_ID=s1&entityTypeId=2&id=1&fieldName=UF_CRM_123456&fileId=10",
                    "urlMachine": "https://your-domain.bitrix24.com/rest/crm.controller.item.getFile.json?auth=***&token=***"
                }
            ]
        }
    }
}
```

The `id` in such a field is the CRM file identifier, not the `ID` of an object on Drive. Drive methods will not return data for this number.

## Download a File from a CRM Timeline Comment {#timeline-comment}

Timeline comment files are returned by the [crm.timeline.comment.get](../crm/timeline/comments/crm-timeline-comment-get.md) and [crm.timeline.comment.list](../crm/timeline/comments/crm-timeline-comment-list.md) methods. In the `FILES` field, the object key matches the file `id`.

```json
{
    "result": {
        "ID": "1",
        "ENTITY_ID": "2",
        "ENTITY_TYPE": "deal",
        "COMMENT": "New comment was added",
        "FILES": {
            "10": {
                "id": 10,
                "type": "file",
                "name": "1.txt",
                "size": 13,
                "urlPreview": null,
                "urlShow": "https://your-domain.bitrix24.com/disk/downloadFile/10/?&ncc=1&filename=1.txt",
                "urlDownload": "https://your-domain.bitrix24.com/disk/downloadFile/10/?&ncc=1&filename=1.txt"
            }
        }
    }
}
```

The `urlDownload` link opens the file in an authorized Bitrix24 context. It does not contain a REST token, so it is not suitable for server-side downloading via a webhook: an HTTP client without browser authorization receives an HTML page instead of the file contents.

To download the file from a server application:

1. Take the file `id` from the `FILES` object
2. Call [disk.file.get](../disk/file/disk-file-get.md) with this `id`
3. Download the file using `DOWNLOAD_URL` from the [disk.file.get](../disk/file/disk-file-get.md) response

## Download a Drive File {#disk}

If the field stores a Drive file, retrieve the file `ID` and call [disk.file.get](../disk/file/disk-file-get.md). The method returns `DOWNLOAD_URL`.

```json
{
    "result": {
        "ID": "10",
        "NAME": "report.docx",
        "TYPE": "file",
        "SIZE": "21668",
        "DOWNLOAD_URL": "https://your-domain.bitrix24.com/rest/download.json?auth=***&token=***",
        "DETAIL_URL": "https://your-domain.bitrix24.com/company/personal/user/1/disk/file/report.docx"
    }
}
```

Some fields store an attachment identifier instead of the file `ID`. For example, task files and some list file fields are linked to an object through an attachment. First call [disk.attachedObject.get](../disk/attached-object/disk-attached-object-get.md), take `OBJECT_ID`, and retrieve `DOWNLOAD_URL` with [disk.file.get](../disk/file/disk-file-get.md).

## Download a File from a List {#lists}

To retrieve the URL of a file from a list item property, call [lists.element.get.file.url](../lists/elements/lists-element-get-file-url.md).

For a property of the "File (Drive)" type, the method returns a download link through the attachment:

```json
{
    "result": [
        "/bitrix/tools/disk/uf.php?attachedId=10&action=download&ncc=1"
    ]
}
```

For a property of the "File" type, the method returns a link to the list file:

```json
{
    "result": [
        "/company/lists/1/file/0/10/PROPERTY_123/20/?ncc=y&download=y"
    ]
}
```

## Download a File from a Task or Feed Post {#attached-files}

Task and feed post files are stored on Drive and linked to the object through an attachment identifier.

The [tasks.task.get](../tasks/tasks-task-get.md) method returns task files in the `UF_TASK_WEBDAV_FILES` field. The value can arrive with the `n` prefix, for example `n491`. For the [disk.attachedObject.get](../disk/attached-object/disk-attached-object-get.md) method, pass the number without the prefix.

```json
{
    "result": {
        "task": {
            "id": 1,
            "ufTaskWebdavFiles": [
                "n10"
            ]
        }
    }
}
```

The [log.blogpost.get](../log/log-blogpost-get.md) method returns attachment identifiers in the `FILES` field.

```json
{
    "result": [
        {
            "ID": 1,
            "FILES": [
                10
            ]
        }
    ]
}
```

To download an individual task or feed post file:

1. Call [disk.attachedObject.get](../disk/attached-object/disk-attached-object-get.md) by attachment identifier
2. Take `OBJECT_ID` from the response
3. Call [disk.file.get](../disk/file/disk-file-get.md) and download the file using `DOWNLOAD_URL`

All task files can be downloaded as an archive. The [tasks.task.get REST v3](../tasks/tasks-task-get-rest-v3.md) method returns the `archiveLink` link.

```json
{
    "result": {
        "item": {
            "id": 1,
            "archiveLink": "/bitrix/tools/disk/uf.php?entityId=1&entity=TASKS_TASK&fieldName=UF_TASK_WEBDAV_FILES&action=downloadArchiveByEntity&ncc=1"
        }
    }
}
```

A feed comment is returned by the [log.blogcomment.user.get](../log/blogcomment/log-blogcomment-user-get.md) method. The `FILES` field contains an object with file data and the `urlDownload` link.

```json
{
    "result": [
        {
            "ID": "1",
            "FILES": {
                "10": {
                    "id": 10,
                    "type": "file",
                    "name": "file.txt",
                    "urlDownload": "https://your-domain.bitrix24.com/disk/downloadFile/10"
                }
            }
        }
    ]
}
```

If a server application needs a signed REST link, pass the file `id` from `FILES` to [disk.file.get](../disk/file/disk-file-get.md) and use `DOWNLOAD_URL`.

## Download a Catalog Product File {#catalog}

The [catalog.product.get](../catalog/product/catalog-product-get.md) and [catalog.product.list](../catalog/product/catalog-product-list.md) methods return product files in image fields and custom properties of the "file" type. The file value contains `id`, `url`, and `urlMachine`.

```json
{
    "result": {
        "products": [
            {
                "id": 1,
                "property123": {
                    "value": {
                        "id": "10",
                        "url": "/rest/catalog.product.download?fields%5BfieldName%5D=property123&fields%5BfileId%5D=10&fields%5BproductId%5D=1",
                        "urlMachine": "/rest/catalog.product.download?fields%5BfieldName%5D=property123&fields%5BfileId%5D=10&fields%5BproductId%5D=1"
                    },
                    "valueId": "20"
                }
            }
        ]
    }
}
```

To download the file, use `urlMachine` or call [catalog.product.download](../catalog/product/catalog-product-download.md). The `catalog.product.download` method returns the file body directly.

## Download a Document Generator Template or Document {#document-generator}

The [documentgenerator.template.get](../document-generator/templates/document-generator-template-get.md), [documentgenerator.template.list](../document-generator/templates/document-generator-template-list.md), [crm.documentgenerator.template.get](../crm/document-generator/templates/crm-document-generator-template-get.md), and [crm.documentgenerator.template.list](../crm/document-generator/templates/crm-document-generator-template-list.md) methods return the `downloadMachine` field.

The [documentgenerator.document.add](../document-generator/document-generator-document-add.md), [documentgenerator.document.list](../document-generator/document-generator-document-list.md), [crm.documentgenerator.document.add](../crm/document-generator/documents/crm-document-generator-document-add.md), and [crm.documentgenerator.document.list](../crm/document-generator/documents/crm-document-generator-document-list.md) methods return the `downloadUrlMachine` field.

```json
{
    "template": {
        "id": 1,
        "downloadMachine": "https://your-domain.bitrix24.com/rest/documentgenerator.api.template.download.json?auth=***&token=***"
    },
    "document": {
        "id": 2,
        "downloadUrlMachine": "https://your-domain.bitrix24.com/rest/documentgenerator.api.document.getfile.json?auth=***&token=***"
    }
}
```

For a template, use `downloadMachine`; for a generated document, use `downloadUrlMachine`.

## Download a Chat File {#chat}

A chat file is downloaded with a separate method depending on the context:

- [im.v2.File.download](../chat-bots/chat-bots-v2/im.v2/files/file-download.md) — for a file on behalf of a user
- [imbot.v2.File.download](../chat-bots/chat-bots-v2/imbot.v2/files/file-download.md) — for a file on behalf of a bot

Both methods return `downloadUrl`.

```json
{
    "result": {
        "downloadUrl": "https://your-domain.bitrix24.com/rest/download.json?auth=***&token=***"
    }
}
```

The `downloadUrl` link is single-use. Retrieve a new link before each download.

## Download a Telephony Call Recording {#telephony}

The [voximplant.statistic.get](../telephony/voximplant/voximplant-statistic-get.md) method returns the call recording in the `CALL_RECORD_URL` field if the recording is attached to the call and is available to the current user.

```json
{
    "result": [
        {
            "ID": "1",
            "CALL_ID": "externalCall.example",
            "PORTAL_USER_ID": "1",
            "CALL_RECORD_URL": "https://your-domain.bitrix24.com/rest/download.json?auth=***&token=***"
        }
    ]
}
```

If `CALL_RECORD_URL` is empty, the call has no available recording. First attach a recording with [telephony.externalCall.attachRecord](../telephony/telephony-external-call-attach-record.md), then retrieve the call statistics again.

## Retrieve Knowledge Base File Metadata {#note}

The [note.file.get](../note/file/note-file-get.md) method returns a file object linked to a Knowledge base document. The response contains metadata and `assetMarkdown`, a ready-made block for inserting the file into the document Markdown.

```json
{
    "result": {
        "item": {
            "id": 10,
            "documentId": 1,
            "name": "file.txt",
            "mimeType": "text/plain",
            "assetMarkdown": "[[file fileId=10]]"
        }
    }
}
```

The method does not return a download link or the file body. To make the file appear on the Knowledge base page, insert `assetMarkdown` into the document content with [note.document.update](../note/document/note-document-update.md).

## Download a Signed Document {#sign}

The [sign.b2e.hcmlink.document.get](../sign/sign-b2e-hcmlink-document-get.md) method returns a link to the signed document file in the `fileUrl` field, while the [sign.b2e.mysafe.tail](../sign/sign-b2e-mysafe-tail.md) and [sign.b2e.personal.tail](../sign/sign-b2e-personal-tail.md) methods return it in the `file_url` field.

```json
{
    "result": {
        "fileUrl": "https://your-domain.bitrix24.com/rest/download.json?auth=***&token=***"
    }
}
```

## How to Download {#request}

The following is an example of downloading a file by a link from a method response.

```bash
curl -L \
  -H "User-Agent: MyIntegration/1.0" \
  -H "Accept: */*" \
  -H "Accept-Language: ru-RU,ru;q=0.9,en;q=0.8" \
  -H "Referer: https://your-domain.bitrix24.com/" \
  -o report.pdf \
  "https://your-domain.bitrix24.com/rest/download.json?auth=***&token=***"
```

Pass the `User-Agent`, `Accept`, `Accept-Language`, and `Referer` headers according to the rules in [How a Request Is Executed](../../settings/how-to-call-rest-api/general-principles.md#headers). If the HTTP client does not pass these headers or substitutes a technical `User-Agent`, the file may fail to download even if the link is signed correctly.

If the method itself returns a file, for example [catalog.product.download](../catalog/product/catalog-product-download.md), save the response body of the REST method as a file. Such a response will not contain JSON with `result`.

Check the HTTP status and response type. If JSON with an error arrives instead of the file, handle the error code: check permissions, the link lifetime, and retrieve the link again before downloading.

If an HTML authorization page arrives instead of the file, the link is not suitable for server-side downloading. Retrieve `urlMachine`, `DOWNLOAD_URL`, `downloadMachine`, `downloadUrlMachine`, or another download field from the table.

## See Also

- [How to Upload Files](./how-to-upload-files.md) — file transfer formats and uploading multiple files to a multiple field

- [How to Update and Delete Files](./how-to-update-files.md) — replacing a file, deleting a file, and retaining the remaining files of a multiple field

- [How to Work with Files](./index.md) — section overview: field types, file relationships with Bitrix24 objects, and main Drive methods
