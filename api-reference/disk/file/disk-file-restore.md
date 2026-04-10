# Restore File from Trash disk.file.restore

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Edit" access permission for the required file

The method `disk.file.restore` restores a file from the trash.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the file located in the trash.

Files in the trash are not accessible through standard methods. To obtain the identifier of the file for restoration, save it immediately after calling the method [disk.file.markdeleted](./disk-file-mark-deleted.md)
||
|#

{% note info "" %}

If there is already a file with the same name in the folder, a number will be automatically added to the name upon restoration. For example, instead of 'New Spreadsheet.xlsx', 'New Spreadsheet (1).xlsx' will be created.

{% endnote %}   

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":9037}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/disk.file.restore
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":9037,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/disk.file.restore
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'disk.file.restore',
            {
                id: 9037,
            }
        );
        
        const result = response.getData().result;
        console.log('File restored:', result);
        
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
                'disk.file.restore',
                [
                    'id' => 9037
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error restoring file: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "disk.file.restore",
        {
            id: 9037
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
        'disk.file.restore',
        [
            'id' => 9037
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
        "ID": "9037",
        "NAME": "picture.png",
        "CODE": null,
        "STORAGE_ID": "1357",
        "TYPE": "file",
        "PARENT_ID": "8930",
        "DELETED_TYPE": 0,
        "GLOBAL_CONTENT_VERSION": "1",
        "FILE_ID": "32933",
        "SIZE": "1679",
        "CREATE_TIME": "2026-02-09T17:35:49+01:00",
        "UPDATE_TIME": "2026-02-16T14:57:24+01:00",
        "DELETE_TIME": "2026-02-16T14:43:38+01:00",
        "CREATED_BY": "1269",
        "UPDATED_BY": "1269",
        "DELETED_BY": "1269",
        "DOWNLOAD_URL": "https://test.bitrix24.com/rest/download.json?auth=845a93690000071b006e2cf2000004f50000076345539a39c27ec694f85bece0eed696&token=disk%7CaWQ9OTAzNyZfPUljQlU8VVAyMldDUG95bW51NlBmUUVVazlPeUd5WUlR%7CImRvd25sb2FkfGRpc2t8YVdROU9UQXpOeVpmUFVsalFsVTFWVkF5TWxkRFVHOTViVzUxTmxCbVVVVlZhemxQZVVkNVdVbFJ8ODQ1YTkzNjkwMDAwMDcxYjAwNmUyY2YyMDAwMDA0ZjUwMDAwMDc2MzQ1NTM5YTM5YzI3ZWM2OTRmODViZWNlMGVlZDY5NiI%3D.axFboRC%2FEf5h1jXPcqNbho7BDEsZqxfVUEf2yL6aqe0%3D",
        "DETAIL_URL": "https://test.bitrix24.com/company/personal/user/1269/disk/file/Folder/Folder in folder/picture (1).png"
    },
    "time": {
        "start": 1771261044,
        "finish": 1771261044.790871,
        "duration": 0.7908709049224854,
        "processing": 0,
        "date_start": "2026-02-16T14:57:24+01:00",
        "date_finish": "2026-02-16T14:57:24+01:00",
        "operating_reset_at": 1771261644,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Array containing file fields ||
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
- `3` — in trash
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
|| `ERROR_ARGUMENT` | Invalid value of parameter {Parameter #0} | Required parameter `id` is missing ||
|| `ERROR_NOT_FOUND` | Could not find entity with id `X` | File with the specified `id` not found ||
|| `ACCESS_DENIED` | Access denied | Insufficient permissions to restore the file ||
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
- [{#T}](./disk-file-upload-version.md)