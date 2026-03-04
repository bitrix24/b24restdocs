# Add File to Chat im.disk.file.commit

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: chat participant

The method `im.disk.file.commit` adds a file to a chat.

To add a file, specify:
- one of the chat identifier parameters — `CHAT_ID` or `DIALOG_ID` 
- one of the file identifier parameters — `FILE_ID` or `UPLOAD_ID`

If multiple parameters are passed simultaneously, the method processes only the first one.

You can obtain the identifier of the new file after uploading it using the method [disk.folder.upload.file](../../disk/folder/disk-folder-upload-file.md). To get the identifier of an existing file, use:
- [disk.storage.getchildren](../../disk/storage/disk-storage-get-children.md) — if the file is located in the root of the storage
- [disk.folder.getchildren](../../disk/folder/disk-folder-get-children.md) — if the file is located in a folder

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CHAT_ID***
[`integer`](../../data-types.md) | Identifier of the chat.

Required if `DIALOG_ID` is not provided ||
|| **DIALOG_ID***
[`string`](../../data-types.md) | Identifier of the dialog in the format:
- `chatXXX` — chat
- `sgXXX` — group or project chat
- `XXX` — user identifier for personal chat

Required if `CHAT_ID` is not provided ||
|| **FILE_ID***
[`integer`](../../data-types.md) | Identifier of the file on Drive. An array can be passed.

Required if `UPLOAD_ID` is not provided ||
|| **UPLOAD_ID***
[`integer`](../../data-types.md) | Identifier of the file on Drive. An array can be passed.

Supports an additional parameter `AS_FILE`, which allows sending the image without compression, as a file.

Required if `FILE_ID` is not provided ||
|| **MESSAGE**
[`string`](../../data-types.md) | Text message with the file ||
|| **SILENT_MODE**
[`string`](../../data-types.md) | Parameter for Open Channels chat

Possible values:
- `Y` — send notification to the client
- `N` — do not send notification to the client ||
|| **AS_FILE**
[`string`](../../data-types.md) | Send as a file. Only for `UPLOAD_ID`.

Possible values:
- `Y` — yes
- `N` — no ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"CHAT_ID":1489,"FILE_ID":[5249,5250],"MESSAGE":"Project documents"}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.disk.file.commit
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"CHAT_ID":1489,"FILE_ID":[5249,5250],"MESSAGE":"Project documents","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.disk.file.commit
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'im.disk.file.commit',
            {
                CHAT_ID: 1489,
                FILE_ID: [5249, 5250],
                MESSAGE: 'Project documents',
            }
        );

        console.log(response.getData().result);
    }
    catch (error)
    {
        console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'im.disk.file.commit',
                [
                    'CHAT_ID' => 1489,
                    'FILE_ID' => [5249, 5250],
                    'MESSAGE' => 'Project documents',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.disk.file.commit',
        {
            CHAT_ID: 1489,
            FILE_ID: [5249, 5250],
            MESSAGE: 'Project documents',
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'im.disk.file.commit',
        [
            'CHAT_ID' => 1489,
            'FILE_ID' => [5249, 5250],
            'MESSAGE' => 'Project documents',
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "FILES": {
            "upload5249": {
                "id": 5249,
                "chatId": 1489,
                "date": {},
                "type": "file",
                "name": "image.png",
                "extension": "png",
                "size": 2144,
                "image": {
                    "height": 61,
                    "width": 72
                },
                "status": "done",
                "progress": 100,
                "authorId": 503,
                "authorName": "John Smith",
                "urlPreview": "https://mysite.com/bitrix/services/main/ajax.php?action=disk.api.file.download&SITE_ID=s1&humanRE=1&fileId=5249&exact=N&_esd=hpbccd%2FZFlCVMvT8%2FoXYU%2FfMrCjiXqxIAf6V4Sv1rR0euQRiW7%2BsdhF7n1QGRL8ZBBmpuiVaX9sY2NbsyigTzBiykXGbFEbXUAmoPO8IvcdVkdoD5n6CJHZG9DZ0DRpH6i5goVbMdjo%3D&fileName=image.png",
                "urlShow": "https://mysite.com/bitrix/services/main/ajax.php?action=disk.api.file.showImage&SITE_ID=s1&humanRE=1&fileId=5249&width=1280&height=1280&signature=9f56cfa3412e55679012a6c3bef9ff391f1fc7becf6dc42bea2b8d68656934ce&exact=N&_esd=hpbccd%2FZFlCVMvT8%2FoXYU%2FfMrCjiXqxIAf6V4Sv1rR0euQRiW7%2BsdhF7n1QGRL8ZBBmpuiVaX9sY2NbsyigTzBiykXGbFEbXUAmoPO8IvcdVkdoD5n6CJHZG9DZ0DRpH6i5goVbMdjo%3D&fileName=image.png",
                "urlDownload": "https://mysite.com/bitrix/services/main/ajax.php?action=disk.api.file.download&SITE_ID=s1&humanRE=1&fileId=5249&exact=N&_esd=hpbccd%2FZFlCVMvT8%2FoXYU%2FfMrCjiXqxIAf6V4Sv1rR0euQRiW7%2BsdhF7n1QGRL8ZBBmpuiVaX9sY2NbsyigTzBiykXGbFEbXUAmoPO8IvcdVkdoD5n6CJHZG9DZ0DRpH6i5goVbMdjo%3D&fileName=image.png",
                "viewerAttrs": {
                    "viewer": "",
                    "viewerType": "image",
                    "src": "https://mysite.com/bitrix/services/main/ajax.php?action=disk.api.file.download&SITE_ID=s1&humanRE=1&fileId=5249&exact=N&_esd=hpbccd%2FZFlCVMvT8%2FoXYU%2FfMrCjiXqxIAf6V4Sv1rR0euQRiW7%2BsdhF7n1QGRL8ZBBmpuiVaX9sY2NbsyigTzBiykXGbFEbXUAmoPO8IvcdVkdoD5n6CJHZG9DZ0DRpH6i5goVbMdjo%3D&fileName=image.png",
                    "viewerResized": "",
                    "objectId": "5249",
                    "viewerGroupBy": "1489",
                    "imChatId": 1489,
                    "title": "image.png",
                    "actions": "[{\"type\":\"download\"},{\"type\":\"copyToMe\",\"text\":\"Save to Drive\",\"action\":\"BXIM.disk.saveToDiskAction\",\"params\":{\"fileId\":\"5249\"},\"extension\":\"disk.viewer.actions\",\"buttonIconClass\":\"ui-btn-icon-cloud\"}]"
                },
                "mediaUrl": {
                    "preview": {
                        "250": "https://mysite.com/bitrix/services/main/ajax.php?action=disk.api.file.download&SITE_ID=s1&humanRE=1&fileId=5249&exact=N&_esd=hpbccd%2FZFlCVMvT8%2FoXYU%2FfMrCjiXqxIAf6V4Sv1rR0euQRiW7%2BsdhF7n1QGRL8ZBBmpuiVaX9sY2NbsyigTzBiykXGbFEbXUAmoPO8IvcdVkdoD5n6CJHZG9DZ0DRpH6i5goVbMdjo%3D&fileName=image.png"
                    }
                },
                "isTranscribable": false,
                "isVideoNote": false,
                "isVoiceNote": false
            }
        },
        "DISK_ID": [
            "5249"
        ],
        "FILE_MODELS": {
            "upload5249": {
                "id": 5249,
                "name": "image.png",
                "createTime": {},
                "updateTime": {},
                "deleteTime": null,
                "code": "media_original",
                "xmlId": null,
                "storageId": 663,
                "realObjectId": 5249,
                "parentId": 4821,
                "deletedType": 0,
                "createdBy": "503",
                "updatedBy": "503",
                "deletedBy": "0",
                "uniqueCode": "k7lj3sQxTRWSi6K93Vyh",
                "typeFile": 2,
                "globalContentVersion": 2,
                "fileId": 57077,
                "size": 2144,
                "etag": "73c045036a9e96943fa57316371655c2",
                "links": {
                    "download": "/bitrix/services/main/ajax.php?action=disk.file.download&SITE_ID=s1&fileId=5249",
                    "showInGrid": "/bitrix/tools/disk/focus.php?objectId=5249&action=showObjectInGrid&ncc=1",
                    "preview": "/bitrix/services/main/ajax.php?action=disk.api.file.showImage&SITE_ID=s1&humanRE=1&width=640&height=640&signature=8e152b3f4820b07a3f8ea79a6de60b0ae5a82a57467d08d1e8a8a399afb0330f&fileId=5249"
                }
            }
        },
        "MESSAGE_ID": 84779
    },
    "time": {
        "start": 1772451339,
        "finish": 1772451339.658828,
        "duration": 0.6588280200958252,
        "processing": 0,
        "date_start": "2026-03-02T14:35:39+01:00",
        "date_finish": "2026-03-02T14:35:39+01:00",
        "operating_reset_at": 1772451939,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root object of the result [(detailed description)](#result-item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Object result-item {#result-item}

#|
|| **Name**
`type` | **Description** ||
|| **FILES**
[`object`](../../data-types.md) | Data of added files [(detailed description)](#files-item) ||
|| **DISK_ID**
[`array`](../../data-types.md) | Array of file identifiers on Drive ||
|| **FILE_MODELS**
[`object`](../../data-types.md) | Models of added files on Drive [(detailed description)](#file-models-item) ||
|| **MESSAGE_ID**
[`integer`](../../data-types.md) | Identifier of the message with files ||
|#

#### Object FILES {#files-item}

#|
|| **Name**
`type` | **Description** ||
|| **upload{id}**
[`object`](../../data-types.md) | File object, where `id` — identifier of the upload file [(detailed description)](#files-upload-item) ||
|#

#### Object FILES.upload{id} {#files-upload-item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the file on Drive ||
|| **chatId**
[`integer`](../../data-types.md) | Identifier of the chat ||
|| **date**
[`object`](../../data-types.md) | Date of file creation ||
|| **type**
[`string`](../../data-types.md) | Type of the item ||
|| **name**
[`string`](../../data-types.md) | Name of the file ||
|| **extension**
[`string`](../../data-types.md) | File extension ||
|| **size**
[`integer`](../../data-types.md) | Size of the file in bytes ||
|| **image**
[`object`](../../data-types.md) | Image parameters [(detailed description)](#files-upload-image) ||
|| **status**
[`string`](../../data-types.md) | Status of file processing ||
|| **progress**
[`integer`](../../data-types.md) | Progress of file processing in percentage ||
|| **authorId**
[`integer`](../../data-types.md) | Identifier of the file author ||
|| **authorName**
[`string`](../../data-types.md) | Name of the file author ||
|| **urlPreview**
[`string`](../../data-types.md) | Link to the file preview ||
|| **urlShow**
[`string`](../../data-types.md) | Link to view the file ||
|| **urlDownload**
[`string`](../../data-types.md) | Link to download the file ||
|| **viewerAttrs**
[`object`](../../data-types.md) | File viewer parameters [(detailed description)](#files-upload-viewer-attrs) ||
|| **mediaUrl**
[`object`](../../data-types.md) | Links to media file [(detailed description)](#files-upload-media-url) ||
|| **isTranscribable**
[`boolean`](../../data-types.md) | Is the file transcribable ||
|| **isVideoNote**
[`boolean`](../../data-types.md) | Is the file a video note ||
|| **isVoiceNote**
[`boolean`](../../data-types.md) | Is the file a voice note ||
|#

#### Object image {#files-upload-image}

#|
|| **Name**
`type` | **Description** ||
|| **height**
[`integer`](../../data-types.md) | Height of the image ||
|| **width**
[`integer`](../../data-types.md) | Width of the image ||
|#

#### Object viewerAttrs {#files-upload-viewer-attrs}

#|
|| **Name**
`type` | **Description** ||
|| **viewer**
[`string`](../../data-types.md) | Viewer identifier ||
|| **viewerType**
[`string`](../../data-types.md) | Type of viewer ||
|| **src**
[`string`](../../data-types.md) | Source file for the viewer ||
|| **viewerResized**
[`string`](../../data-types.md) | Source of the reduced version of the file ||
|| **objectId**
[`string`](../../data-types.md) | Identifier of the object in the viewer ||
|| **viewerGroupBy**
[`string`](../../data-types.md) | Identifier of the viewer group ||
|| **imChatId**
[`integer`](../../data-types.md) | Identifier of the chat for the viewer ||
|| **title**
[`string`](../../data-types.md) | Title in the viewer ||
|| **actions**
[`string`](../../data-types.md) | List of actions in the viewer in JSON string format ||
|#

#### Object mediaUrl {#files-upload-media-url}

#|
|| **Name**
`type` | **Description** ||
|| **preview**
[`object`](../../data-types.md) | Set of links to file previews by size [(detailed description)](#files-upload-media-url-preview) ||
|#

#### Object mediaUrl.preview {#files-upload-media-url-preview}

#|
|| **Name**
`type` | **Description** ||
|| **250**
[`string`](../../data-types.md) | Link to preview with a width of 250 px ||
|#

#### Object FILE_MODELS {#file-models-item}

#|
|| **Name**
`type` | **Description** ||
|| **upload{id}**
[`object`](../../data-types.md) | File model object, where `id` — identifier of the upload file [(detailed description)](#file-models-upload-item) ||
|#

#### Object FILE_MODELS.upload{id} {#file-models-upload-item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the file on Drive ||
|| **name**
[`string`](../../data-types.md) | Name of the file ||
|| **createTime**
[`object`](../../data-types.md) | Date of file creation ||
|| **updateTime**
[`object`](../../data-types.md) | Date of file update ||
|| **deleteTime**
[`string`](../../data-types.md) | Date of file deletion, can be `null` ||
|| **code**
[`string`](../../data-types.md) | File type code ||
|| **xmlId**
[`string`](../../data-types.md) | External identifier, can be `null` ||
|| **storageId**
[`integer`](../../data-types.md) | Identifier of the storage ||
|| **realObjectId**
[`integer`](../../data-types.md) | Identifier of the real object ||
|| **parentId**
[`integer`](../../data-types.md) | Identifier of the parent folder ||
|| **deletedType**
[`integer`](../../data-types.md) | Deletion type ||
|| **createdBy**
[`string`](../../data-types.md) | Identifier of the creator ||
|| **updatedBy**
[`string`](../../data-types.md) | Identifier of the updater ||
|| **deletedBy**
[`string`](../../data-types.md) | Identifier of the deleter ||
|| **uniqueCode**
[`string`](../../data-types.md) | Unique code of the file ||
|| **typeFile**
[`integer`](../../data-types.md) | Numeric code of the file type ||
|| **globalContentVersion**
[`integer`](../../data-types.md) | Global content version ||
|| **fileId**
[`integer`](../../data-types.md) | Identifier of the related file ||
|| **size**
[`integer`](../../data-types.md) | Size of the file in bytes ||
|| **etag**
[`string`](../../data-types.md) | ETag of the file ||
|| **links**
[`object`](../../data-types.md) | Links for working with the file [(detailed description)](#file-models-upload-links) ||
|#

#### Object links {#file-models-upload-links}

#|
|| **Name**
`type` | **Description** ||
|| **download**
[`string`](../../data-types.md) | Link to download the file ||
|| **showInGrid**
[`string`](../../data-types.md) | Link to show the file in the grid ||
|| **preview**
[`string`](../../data-types.md) | Link to preview the file ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "CHAT_ID_EMPTY",
    "error_description": "Chat ID can't be empty"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `CHAT_ID_EMPTY` | Chat ID can't be empty | Possible reasons:
- one of the required parameters `CHAT_ID` or `DIALOG_ID` is not provided
- empty `CHAT_ID` is passed ||
|| `400` | `DIALOG_ID_EMPTY` | Dialog ID can't be empty | Empty or invalid `DIALOG_ID` is passed ||
|| `400` | `FILES_ERROR` | List of files is not specified | One of the required parameters `FILE_ID` or `UPLOAD_ID` is not provided ||
|| `400` | `SAVE_ERROR` | Error during saving file to chat | Possible reasons:
- `FILE_ID` or `UPLOAD_ID` is passed empty
- non-existent file identifiers are passed ||
|| `403` | `ACCESS_ERROR` | You do not have access to the specified dialog | Insufficient rights to view the dialog or a non-existent dialog is passed ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-disk-file-save.md)
- [{#T}](./im-disk-file-delete.md)
- [{#T}](./im-disk-folder-get.md)