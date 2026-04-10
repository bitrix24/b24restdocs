# Move File to Trash disk.file.markdeleted

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Edit" access permission for the specified file

The method `disk.file.markdeleted` moves a file to the trash.

{% note info "" %}

Save the file ID after deletion so that it can be restored later using the [disk.file.restore](./disk-file-restore.md) method.

{% endnote %} 

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | File identifier.

The identifier can be obtained using the [disk.storage.getchildren](../storage/disk-storage-get-children.md) method if the file is located at the root of the storage, and using the [disk.folder.getchildren](../folder/disk-folder-get-children.md) method if the file is located in a folder.
||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":9037}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/disk.file.markdeleted
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":9037,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/disk.file.markdeleted
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'disk.file.markdeleted',
            {
                id: 9037,
            }
        );
        
        const result = response.getData().result;
        console.log('File marked as deleted:', result);
        
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
                'disk.file.markdeleted',
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
        echo 'Error marking file as deleted: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "disk.file.markdeleted",
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
        'disk.file.markdeleted',
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
        "DELETED_TYPE": "3",
        "GLOBAL_CONTENT_VERSION": "1",
        "FILE_ID": "32933",
        "SIZE": "1679",
        "CREATE_TIME": "2026-02-09T17:35:49+01:00",
        "UPDATE_TIME": "2026-02-09T17:35:49+01:00",
        "DELETE_TIME": "2026-02-16T14:43:38+01:00",
        "CREATED_BY": "1269",
        "UPDATED_BY": "1269",
        "DELETED_BY": "1269",
        "DOWNLOAD_URL": "https://test.bitrix24.com/rest/download.json?auth=904993690000071b006e2cf2000004f5000007bb5f672541f2cec7c7f0b65135f70180&token=disk%7CaWQ9OTAzNyZfPWVNNEllSjUwZEV7OUN1aEhETzBobFdDOWlMSEFZNk5x%7CImRvd25sb2FkfGRpc2t8YVdROU9UQXpOeVpmUFdWTk5FbGxTalV3WkVWMk9VTjFhRWhFVHpCb2JGZERPV9xNU0VGWk5rNXh8OTA0OTkzNjkwMDAwMDcxYjAwNmUyY2YyMDAwMDA0ZjUwMDAwMDdiYjVmNjcyNTQxZjJjZWM3YzdmMGI2NTEzNWY3MDE4MCI%3D.nSSCKa7KdIxa0ToaCO31FIV3VZwvFxUTR1Mpl39508Y%3D",
        "DETAIL_URL": "https://test.bitrix24.com/company/personal/user/1269/disk/file/Folder/Folder in folder/picture.png"
    },
    "time": {
        "start": 1771260218,
        "finish": 1771260218.490546,
        "duration": 0.49054598808288574,
        "processing": 0,
        "date_start": "2026-02-16T14:43:38+01:00",
        "date_finish": "2026-02-16T14:43:38+01:00",
        "operating_reset_at": 1771260818,
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
|| `ERROR_ARGUMENT` | Invalid value of parameter {Parameter #0} | Required parameter `id` is missing ||
|| `ERROR_NOT_FOUND` | Could not find entity with id `X` | File with the specified `id` not found ||
|| `ACCESS_DENIED` | Access denied | Insufficient permissions to move the file to the trash ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./disk-file-copy-to.md)
- [{#T}](./disk-file-delete.md)
- [{#T}](./disk-file-get-external-link.md)
- [{#T}](./disk-file-get-fields.md)
- [{#T}](./disk-file-get-versions.md)
- [{#T}](./disk-file-get.md)
- [{#T}](./disk-file-move-to.md)
- [{#T}](./disk-file-rename.md)
- [{#T}](./disk-file-restore-from-version.md)
- [{#T}](./disk-file-restore.md)
- [{#T}](./disk-file-upload-version.md)