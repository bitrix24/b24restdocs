# Rename File disk.file.rename

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Edit" access permission for the specified file

The method `disk.file.rename` renames a file.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | File identifier.

The identifier can be obtained using the method [disk.storage.getchildren](../storage/disk-storage-get-children.md) if the file is located at the root of the storage, and using the method [disk.folder.getchildren](../folder/disk-folder-get-children.md) if the file is located in a folder ||
|| **newName***
[`string`](../../data-types.md) | New file name.

The file name must be provided along with the extension corresponding to its format. If you use a name without an extension or with an extension that does not match the original, the file will be corrupted ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8964,"newName":"New File Name.png"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/disk.file.rename
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8964,"newName":"New File Name.png","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/disk.file.rename
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'disk.file.rename',
            {
                id: 8964,
                newName: 'New File Name.png'
            }
        );
        
        const result = response.getData().result;
        console.log('Renamed file with ID:', result);
        
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
                'disk.file.rename',
                [
                    'id' => 8964,
                    'newName' => 'New File Name.png'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error renaming file: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "disk.file.rename",
        {
            id: 8964,
            newName: 'New File Name.png'
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
        'disk.file.rename',
        [
            'id' => 8964,
            'newName' => 'New File Name.png'
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
        "NAME": "New File Name.png",
        "CODE": null,
        "STORAGE_ID": "1357",
        "TYPE": "file",
        "PARENT_ID": "9023",
        "DELETED_TYPE": "0",
        "GLOBAL_CONTENT_VERSION": "1",
        "FILE_ID": "32718",
        "SIZE": "52486",
        "CREATE_TIME": "2026-01-14T17:05:05+01:00",
        "UPDATE_TIME": "2026-02-17T11:59:55+01:00",
        "DELETE_TIME": null,
        "CREATED_BY": "1269",
        "UPDATED_BY": "1269",
        "DELETED_BY": "0",
        "DOWNLOAD_URL": "https://test.bitrix24.com/rest/download.json?auth=343794690000071b006e2cf2000004f500000746484b1f82771b3434ff80eb15edc8f8&token=disk%7CaWQ9ODk2NCZfPXU2V3NhdUVHeXBsY2thekFuTjB3ekNWMFd6d3dwVEZa%7CImRvd25sb2FkfGRpc2t8YVdROU9EazJOQ1pmUFhVMlYzTmhkVVZIZVhCc1kydGhla0Z1VGpCM2VrTldNRnQ2ZDNkd1ZFWmF8MzQzNzk0NjkwMDAwMDcxYjAwNmUyY2YyMDAwMDA0ZjUwMDAwMDc0NjQ4NGIxZjgyNzcxYjM0MzRmZjgwZWIxNWVkYzhmOCI%3D.6Rmg3D5ED7iWrkUSMB7E1%2FTrnlxTtQ3bf8H6drXRVM4%3D",
        "DETAIL_URL": "https://test.bitrix24.com/company/personal/user/1269/disk/file/New File Name.png"
    },
    "time": {
        "start": 1771318795,
        "finish": 1771318795.11888,
        "duration": 0.11888003349304199,
        "processing": 0,
        "date_start": "2026-02-17T11:59:55+01:00",
        "date_finish": "2026-02-17T11:59:55+01:00",
        "operating_reset_at": 1771319395,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Array with file fields ||
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
|| `ERROR_ARGUMENT` | Invalid value of parameter {Parameter #0} | Required parameter `id` or `newName` is missing ||
|| `ERROR_NOT_FOUND` | Could not find entity with id `X` | File with the specified `id` not found ||
|| `DISK_OBJ_22000` | A file with that name already exists | A file with that name already exists ||
||  `0` | Empty name | An empty file name was provided ||
|| `ACCESS_DENIED` | Access denied | Insufficient permissions to rename the file ||
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
- [{#T}](./disk-file-restore-from-version.md)
- [{#T}](./disk-file-restore.md)
- [{#T}](./disk-file-upload-version.md)