# Upload a New Version of a File `disk.file.uploadversion`

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Edit" access permission for the specified file

The method `disk.file.uploadversion` uploads a new version of a file.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | File identifier.

The identifier can be obtained using the method [disk.storage.getchildren](../storage/disk-storage-get-children.md) if the file is located at the root of the storage, and using the method [disk.folder.getchildren](../folder/disk-folder-get-children.md) if the file is located in a folder ||
|| **fileContent***
[`array`](../../data-types.md) | An array containing the file name and a string in [Base64](../../files/how-to-upload-files.md) ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":9043,"fileContent":["Test #2.docx","UEsDBBQABgAIAAAAIQBKvAJxbQEAACgGAAATAAgCW0NvbnRlbnRfVHlwZXNdLnhtbCCiBAI...AAAAAA0ADQBAAwAAA2EAAAAA"]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/disk.file.uploadversion
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":9043,"fileContent":["Test #2.docx","UEsDBBQABgAIAAAAIQBKvAJxbQEAACgGAAATAAgCW0NvbnRlbnRfVHlwZXNdLnhtbCCiBAI...AAAAAA0ADQBAAwAAA2EAAAAA"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/disk.file.uploadversion
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'disk.file.uploadversion',
            {
                id: 9043,
                fileContent: [
                    'Test #2.docx',
                    'UEsDBBQABgAIAAAAIQBKvAJxbQEAACgGAAATAAgCW0NvbnRlbnRfVHlwZXNdLnhtbCCiBAI...AAAAAA0ADQBAAwAAA2EAAAAA',
                ]
            }
        );
        
        const result = response.getData().result;
        console.log('Uploaded file version with ID:', result);
        
        processResult(result);
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'disk.file.uploadversion',
                [
                    'id' => 9043,
                    'fileContent' => [
                        'Test #2.docx',
                        'UEsDBBQABgAIAAAAIQBKvAJxbQEAACgGAAATAAgCW0NvbnRlbnRfVHlwZXNdLnhtbCCiBAI...AAAAAA0ADQBAAwAAA2EAAAAA',
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error uploading file version: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "disk.file.uploadversion",
        {
            id: 9043,
            fileContent: [
            'Test #2.docx',
            'UEsDBBQABgAIAAAAIQBKvAJxbQEAACgGAAATAAgCW0NvbnRlbnRfVHlwZXNdLnhtbCCiBAI...AAAAAA0ADQBAAwAAA2EAAAAA',
            ]
        },
        function (result)
        {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'disk.file.uploadversion',
        [
            'id' => 9043,
            'fileContent' => [
                'Test #2.docx',
                'UEsDBBQABgAIAAAAIQBKvAJxbQEAACgGAAATAAgCW0NvbnRlbnRfVHlwZXNdLnhtbCCiBAI...AAAAAA0ADQBAAwAAA2EAAAAA',
            ]
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
        "ID": "9043",
        "NAME": "Test #2.docx",
        "CODE": null,
        "STORAGE_ID": "1357",
        "TYPE": "file",
        "PARENT_ID": "8996",
        "DELETED_TYPE": "0",
        "GLOBAL_CONTENT_VERSION": 7,
        "FILE_ID": "32987",
        "SIZE": "25689",
        "CREATE_TIME": "2026-02-16T12:52:05+01:00",
        "UPDATE_TIME": "2026-02-17T13:26:58+01:00",
        "DELETE_TIME": null,
        "CREATED_BY": "1269",
        "UPDATED_BY": "1269",
        "DELETED_BY": "0",
        "DOWNLOAD_URL": "https://test.bitrix24.com/rest/download.json?auth=815094690000071b006e2cf2000004f5000007a108900724af0be51c3d5962bfd124ef&token=disk%7CaWQ9OTA0MyZfPW8zbFN4MU5lU3pFbHpLSm5Ub1JhZzRJRzFnN2FnYldE%7CImRvd25sb2FkfGRpc2t8YVdROU9UQTBNeVpmUFc4emJGTjRNVTVsVTNwRmJIcExTbTVVYjFKaFp6UkpSekZuTjJGbllsZEV8ODE1MDk0NjkwMDAwMDcxYjAwNmUyY2YyMDAwMDA0ZjUwMDAwMDdhMTA4OTAwNzI0YWYwYmU1MWMzZDU5NjJiZmQxMjRlZiI%3D.yD9KHiasOQNb0ymK6NQjHTOCjDsSbYn%2FTAkju8323fc%3D",
        "DETAIL_URL": "https://test.bitrix24.com/company/personal/user/1269/disk/file/Folder/Folder/Test #2.docx"
    },
    "time": {
        "start": 1771324018,
        "finish": 1771324018.554412,
        "duration": 0.5544118881225586,
        "processing": 0,
        "date_start": "2026-02-17T13:26:58+01:00",
        "date_finish": "2026-02-17T13:26:58+01:00",
        "operating_reset_at": 1771324618,
        "operating": 0.2871270179748535
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | An array containing file fields ||
|| **ID**
[`integer`](../../data-types.md) | File identifier ||
|| **NAME**
[`string`](../../data-types.md) | File name ||
|| **CODE**
[`string`](../../data-types.md) | Symbolic code of the file ||
|| **STORAGE_ID**
[`integer`](../../data-types.md) | Identifier of the storage where the file is located ||
|| **TYPE**
[`enum`](../../data-types.md) | Object type ||
|| **PARENT_ID**
[`integer`](../../data-types.md) | Identifier of the parent folder ||
|| **DELETED_TYPE**
[`enum`](../../data-types.md) | Deletion status of the object. Possible values:
- `0` — not deleted
- `3` — in the trash
- `4` — deleted along with the parent folder ||
|| **GLOBAL_CONTENT_VERSION**
[`integer`](../../data-types.md) | Incremental version counter of the file ||
|| **FILE_ID**
[`integer`](../../data-types.md) | Internal value of the file identifier ||
|| **SIZE**
[`integer`](../../data-types.md) | Size of the file in bytes ||
|| **CREATE_TIME**
[`datetime`](../../data-types.md) | Date and time of file creation ||
|| **UPDATE_TIME**
[`datetime`](../../data-types.md) | Date and time of the last update of the file ||
|| **DELETE_TIME**
[`datetime`](../../data-types.md) | Date and time of moving the file to the trash ||
|| **CREATED_BY**
[`integer`](../../data-types.md) | Identifier of the user who created the file ||
|| **UPDATED_BY**
[`integer`](../../data-types.md) | Identifier of the user who made the last change ||
|| **DELETED_BY**
[`integer`](../../data-types.md) | Identifier of the user who deleted the file ||
|| **DOWNLOAD_URL**
[`string`](../../data-types.md) | Link to download the file ||
|| **DETAIL_URL**
[`string`](../../data-types.md) | Link to open the file in the interface ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":"ERROR_ARGUMENT",
    "error_description":"Invalid value of parameter {Parameter #0}"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_ARGUMENT` | Invalid value of parameter {Parameter #0} | Required parameter `id` or `fileContent` is missing ||
|| `ERROR_NOT_FOUND` | Could not find entity with id `X` | File with the specified `id` not found ||
|| `DISK_FILE_22002` | Could not save file | Failed to save the file. Check available space on the Drive and the correctness of data encoding ||
|| `DISK_FILE_22005` | Could not get saved file | Failed to retrieve data of the saved file ||
|| `DISK_FILE_22006` | Size restriction | The size of the uploaded file exceeds the storage limit ||
|| `ACCESS_DENIED` | Access denied | Insufficient permissions to upload a new version of the file ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./disk-file-copy-to.md)
- [{#T}](./disk-file-delete.md)
- [{#T}](./disk-file-get-external-link.md)
- [{#T}](./disk-file-get-fields.md)
- [{#T}](./disk-file-get-versions.md)
- [{#T}](./disk-file-get.md)
- [{#T}](./disk-file-mark-deleted.md)
- [{#T}](./disk-file-move-to.md)
- [{#T}](./disk-file-rename.md)
- [{#T}](./disk-file-restore-from-version.md)
- [{#T}](./disk-file-restore.md)