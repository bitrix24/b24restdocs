# Move File to Specified Folder disk.file.moveto

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Edit" permission for the file and "Add" permission for the target folder.

The method `disk.file.moveto` moves a file to the specified folder.

{% note warning "" %}

You cannot move a file to a folder from a different storage.

{% endnote %} 

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the file ||
|| **targetFolderId***
[`integer`](../../data-types.md) | Identifier of the folder to which the file should be moved ||
|#

{% note info "" %}

File and folder identifiers can be obtained using the [disk.storage.getchildren](../storage/disk-storage-get-children.md) or [disk.folder.getchildren](../folder/disk-folder-get-children.md) methods.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8964,"targetFolderId":9023}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/disk.file.moveto
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8964,"targetFolderId":9023,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/disk.file.moveto
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'disk.file.moveto',
            {
                id: 8964,
                targetFolderId: 9023
            }
        );
        
        const result = response.getData().result;
        console.log('Moved file with ID:', result);
        
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
                'disk.file.moveto',
                [
                    'id' => 8964,
                    'targetFolderId' => 9023
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error moving file: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "disk.file.moveto",
        {
            id: 8964,
            targetFolderId: 9023
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
        'disk.file.moveto',
        [
            'id' => 8964,
            'targetFolderId' => 9023
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
        "ID": "8964",
        "NAME": "Image.png",
        "CODE": null,
        "STORAGE_ID": "1357",
        "TYPE": "file",
        "PARENT_ID": 9023,
        "DELETED_TYPE": "0",
        "GLOBAL_CONTENT_VERSION": "1",
        "FILE_ID": "32718",
        "SIZE": "52486",
        "CREATE_TIME": "2026-01-14T17:05:05+02:00",
        "UPDATE_TIME": "2026-01-14T17:05:39+02:00",
        "DELETE_TIME": null,
        "CREATED_BY": "1269",
        "UPDATED_BY": "1269",
        "DELETED_BY": "0",
        "DOWNLOAD_URL": "https://test.bitrix24.com/rest/download.json?auth=343794690000071b006e2cf2000004f500000746484b1f82771b3434ff80eb15edc8f8&token=disk%7CaWQ9ODk2NCZfPW1vbGtSSzFIQ25HclVzaDRldXdkTFhoT215M2t5UmhF%7CImRvd25sb2FkfGRpc2t8YVdROU9EazJOQ1pmUFcxdmJHdFNTekZJUTI1SGNsVnphRFJsZFhka1RGaG9UMjE1TTJ0NVVtaEZ8MzQzNzk0NjkwMDAwMDcxYjAwNmUyY2YyMDAwMDA0ZjUwMDAwMDc0NjQ4NGIxZjgyNzcxYjM0MzRmZjgwZWIxNWVkYzhmOCI%3D.0NfJeP8vn%2BqE9%2B5v9Crsh3W%2Bcf6HKgdfKQVTm9z0dto%3D",
        "DETAIL_URL": "https://test.bitrix24.com/company/personal/user/1269/disk/file/Image.png"
    },
    "time": {
        "start": 1771317625,
        "finish": 1771317625.814002,
        "duration": 0.8140020370483398,
        "processing": 0,
        "date_start": "2026-02-17T11:40:25+02:00",
        "date_finish": "2026-02-17T11:40:25+02:00",
        "operating_reset_at": 1771318225,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Array containing file fields.

Returns `false` if the file and folder are in different storages. ||
|| **ID**
[`integer`](../../data-types.md) | Identifier of the file ||
|| **NAME**
[`string`](../../data-types.md) | Name of the file ||
|| **CODE**
[`string`](../../data-types.md) | Symbolic code of the file ||
|| **STORAGE_ID**
[`integer`](../../data-types.md) | Identifier of the storage where the file is located ||
|| **TYPE**
[`enum`](../../data-types.md) | Type of the object ||
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
[`datetime`](../../data-types.md) | Date and time of the last file update ||
|| **DELETE_TIME**
[`datetime`](../../data-types.md) | Date and time the file was moved to the trash ||
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
[`time`](../../data-types.md#time) | Information about the request execution time ||
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
|| `ERROR_ARGUMENT` | Invalid value of parameter {Parameter #0} | Required parameter `id` or `targetFolderId` is missing. ||
|| `ERROR_NOT_FOUND` | Could not find entity with id `X` | File with the specified `id` or folder with the specified `targetFolderId` not found. ||
|| `DISK_OBJ_22000` | A file with this name already exists | A file with this name already exists. ||
|| `DISK_FILE_22007` | Exclusive lock | The file is open for editing by another user. ||
|| `ACCESS_DENIED` | Access denied | Insufficient permissions to move the file. ||
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
- [{#T}](./disk-file-rename.md)
- [{#T}](./disk-file-restore-from-version.md)
- [{#T}](./disk-file-restore.md)
- [{#T}](./disk-file-upload-version.md)