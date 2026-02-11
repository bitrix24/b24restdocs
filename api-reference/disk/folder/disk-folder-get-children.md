# Get a list of files and folders in the folder disk.folder.getchildren

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.folder.getchildren` returns a list of files and folders located in the folder.

{% note info "" %}

Only those files and folders for which the user has "Read" access permission are returned.

{% endnote %}

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the folder.

The identifier can be obtained using the method [disk.storage.getchildren](../storage/disk-storage-get-children.md) if the folder is located at the root of the storage, and using the method [disk.folder.getchildren](./disk-folder-get-children.md) if the folder is located in another folder ||
|| **filter**
[`array`](../../data-types.md) | Array format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```

where:
- `field_n` — the name of the field by which filtering will be performed
- `value_n` — the filter value

You can add a prefix to the keys `field_n` to specify the filter operation.
Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` symbol in the filter value should not be passed. The search looks for a substring at any position in the string
- `=%` — LIKE, substring search. The `%` symbol should be passed in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be at any position
- `%=` — LIKE (similar to `=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal

The list of fields available for filtering can be obtained using the method [disk.folder.getfields](./disk-folder-get-fields.md) ||
|| **order**
[`array`](../../data-types.md) | Array format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```

where:
- `field_n` — the name of the field by which sorting will be performed
- `value_n` — a `string` value equal to:
    - `ASC` — ascending sort
    - `DESC` — descending sort

The list of fields available for sorting can be obtained using the method [disk.folder.getfields](./disk-folder-get-fields.md) ||
|| **start**
[`integer`](../../data-types.md) | This parameter is used to control pagination.

The page size of results is always static — 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N - 1) * 50`, where `N` — the desired page number ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8907,"filter":{">=CREATE_TIME":"2026-01-12"},"order":{"NAME":"DESC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/disk.folder.getchildren
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8907,"filter":{">=CREATE_TIME":"2026-01-12"},"order":{"NAME":"DESC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/disk.folder.getchildren
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'disk.folder.getchildren',
            {
                id: 8907,
                filter: {
                    '>=CREATE_TIME': '2026-01-12'
                },
                order: {
                    NAME: 'DESC'
                }
            }
        );
        
        const result = response.getData().result;
        console.log('Data:', result);
        
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
                'disk.folder.getchildren',
                [
                    'id' => 8907,
                    'filter' => [
                        '>=CREATE_TIME' => '2026-01-12'
                    ],
                    'order' => [
                        'NAME' => 'DESC'
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
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "disk.folder.getchildren",
        {
            id: 8907,
            filter: {
                '>=CREATE_TIME': '2026-01-12'
            },
            order: {
                NAME: 'DESC'
            }
        },
        function (result) {
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
        'disk.folder.getchildren',
        [
            'id' => 8907,
            'filter' => [
                '>=CREATE_TIME' => '2026-01-12'
            ],
            'order' => [
                'NAME' => 'DESC'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": [
        {
            "ID": "8930",
            "NAME": "Folder in Folder",
            "CODE": null,
            "STORAGE_ID": "1357",
            "TYPE": "folder",
            "REAL_OBJECT_ID": "8930",
            "PARENT_ID": "8907",
            "DELETED_TYPE": "0",
            "CREATE_TIME": "2026-01-13T16:16:35+02:00",
            "UPDATE_TIME": "2026-01-13T16:16:35+02:00",
            "DELETE_TIME": null,
            "CREATED_BY": "1269",
            "UPDATED_BY": "1269",
            "DELETED_BY": "0",
            "DETAIL_URL": "https://test.bitrix24.com/company/personal/user/1269/disk/path/Folder/Folder in Folder"
        },
        {
            "ID": "8964",
            "NAME": "Image.png",
            "CODE": null,
            "STORAGE_ID": "1357",
            "TYPE": "file",
            "PARENT_ID": "8907",
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
            "DOWNLOAD_URL": "https://test.bitrix24.com/rest/download.json?auth=d9c467690000071b006e2cf2000004f5000007248f2adc44d050ace99adb3cb9d0f1aa&token=disk%7CaWQ9ODk2NCZfPU9zTE4wUFNMRVBacFJiZXF6Q203dkY4d3V6ZUQyd0Rt%7CImRvd25sb2FkfGRpc2t8YVdROU9EazJOQ1pmUFU5elRFNHdVRk5NUlZCYWFGSmlaWEY2UTIwM2RrWTRkM1Y2WlVReWQwUnR8ZDljNDY3NjkwMDAwMDcxYjAwNmUyY2YyMDAwMDA0ZjUwMDAwMDcyNDhmMmFkYzQ0ZDA1MGFjZTk5YWRiM2NiOWQwZjFhYSI%3D.oSqXbtR%2FjZL8%2BfY%2BUvgqYQdxoHVh7PCvocXUvtS9n4s%3D",
            "DETAIL_URL": "https://test.bitrix24.com/company/personal/user/1269/disk/file/Folder/Image.png"
        },
        {
            "ID": "8936",
            "NAME": "Documents",
            "CODE": null,
            "STORAGE_ID": "1357",
            "TYPE": "folder",
            "REAL_OBJECT_ID": "8936",
            "PARENT_ID": "8907",
            "DELETED_TYPE": "0",
            "CREATE_TIME": "2026-01-13T17:00:40+02:00",
            "UPDATE_TIME": "2026-01-14T17:05:25+02:00",
            "DELETE_TIME": null,
            "CREATED_BY": "1271",
            "UPDATED_BY": "1271",
            "DELETED_BY": "0",
            "DETAIL_URL": "https://test.bitrix24.com/company/personal/user/1269/disk/path/Folder/Documents"
        }
    ],
    "total": 3,
    "time": {
        "start": 1768407161,
        "finish": 1768407161.323201,
        "duration": 0.32320094108581543,
        "processing": 0,
        "date_start": "2026-01-14T17:12:41+02:00",
        "date_finish": "2026-01-14T17:12:41+02:00",
        "operating_reset_at": 1768407761,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | A list of files and folders with field descriptions.

An empty array means that the user does not have permission to view the files and folders located in the specified folder ||
|| **ID**
[`integer`](../../data-types.md) | Identifier of the file/folder ||
|| **NAME**
[`string`](../../data-types.md) | Name of the file/folder ||
|| **CODE**
[`string`](../../data-types.md) | Symbolic code of the file/folder ||
|| **STORAGE_ID**
[`integer`](../../data-types.md) | Identifier of the storage where the file/folder is located ||
|| **TYPE**
[`enum`](../../data-types.md) | Type of the object ||
|| **REAL_OBJECT_ID**
[`integer`](../../data-types.md) | Identifier of the object ||
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
[`datetime`](../../data-types.md) | Date and time of creation of the file/folder ||
|| **UPDATE_TIME**
[`datetime`](../../data-types.md) | Date and time of the last update of the file/folder ||
|| **DELETE_TIME**
[`datetime`](../../data-types.md) | Date and time of moving the file/folder to the trash ||
|| **CREATED_BY**
[`integer`](../../data-types.md) | Identifier of the user who created the file/folder ||
|| **UPDATED_BY**
[`integer`](../../data-types.md) | Identifier of the user who made the last change ||
|| **DELETED_BY**
[`integer`](../../data-types.md) | Identifier of the user who deleted the file/folder ||
|| **DOWNLOAD_URL**
[`string`](../../data-types.md) | Link to download the file ||
|| **DETAIL_URL**
[`string`](../../data-types.md) | Link to open the file/folder in the interface ||
|| **total**
[`integer`](../../data-types.md) | Total number of files and folders ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"ERROR_ARGUMENT",
    "error_description":"Invalid value of parameter {Parameter #1}"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_ARGUMENT` | Invalid value of parameter {Parameter #1} | The required parameter `id` is not specified ||
|| `ERROR_NOT_FOUND` | Could not find entity with id `X` | The folder with the specified `id` was not found ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./disk-folder-add-subfolder.md)
- [{#T}](./disk-folder-copy-to.md)
- [{#T}](./disk-folder-delete-tree.md)
- [{#T}](./disk-folder-get-external-link.md)
- [{#T}](./disk-folder-get-fields.md)
- [{#T}](./disk-folder-get.md)
- [{#T}](./disk-folder-mark-deleted.md)
- [{#T}](./disk-folder-move-to.md)
- [{#T}](./disk-folder-rename.md)
- [{#T}](./disk-folder-restore.md)
- [{#T}](./disk-folder-share-to-user.md)
- [{#T}](./disk-folder-upload-file.md)