# Get a list of files and folders in the root of the storage disk.storage.getchildren

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.storage.getchildren` returns a list of files and folders located in the root of the storage.

{% note info "" %}

Only those files and folders for which the user has "Read" access permission are returned.

{% endnote %}

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the storage.

The identifier can be obtained using the method [disk.storage.getlist](./disk-storage-get-list.md) ||
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
- `field_n` — the name of the field to filter by
- `value_n` — the filter value

You can add a prefix to the keys `field_n` to specify the filter operation.
Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search looks for the substring in any position of the string
- `=%` — LIKE, substring search. The `%` symbol must be passed in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal

The list of fields available for filtering can be obtained using the method [disk.folder.getfields](../folder/disk-folder-get-fields.md) ||
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
- `field_n` — the name of the field to sort by
- `value_n` — a `string` value equal to:
    - `ASC` — ascending sort
    - `DESC` — descending sort

The list of fields available for sorting can be obtained using the method [disk.folder.getfields](../folder/disk-folder-get-fields.md) ||
|| **start**
[`integer`](../../data-types.md) | This parameter is used to manage pagination.

The page size of results is always static — 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N - 1) * 50`, where `N` — the number of the desired page ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1357,"filter":{"NAME":"%Folder%"},"order":{"NAME":"DESC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/disk.storage.getchildren
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1357,"filter":{"NAME":"%Folder%"},"order":{"NAME":"DESC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/disk.storage.getchildren
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'disk.storage.getchildren',
            {
                id: 1357,
                filter: {
                    NAME: '%Folder%',
                },
                order: {
                    NAME: 'DESC',
                }
            }
        );
        
        const result = response.getData().result;
        console.log('Retrieved children:', result);
        
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
                'disk.storage.getchildren',
                [
                    'id' => 1357,
                    'filter' => [
                        'NAME' => '%Folder%'
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
        echo 'Error retrieving children: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "disk.storage.getchildren",
        {
            id: 1357,
            filter: {
                NAME: '%Folder%'
            },
            order: {
                NAME: 'DESC'
            }
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
        'disk.storage.getchildren',
        [
            'id' => 1357,
            'filter' => [
                'NAME' => '%Folder%'
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
            "ID": "8960",
            "NAME": "Folder in folder",
            "CODE": null,
            "STORAGE_ID": "1357",
            "TYPE": "folder",
            "REAL_OBJECT_ID": "8960",
            "PARENT_ID": "8875",
            "DELETED_TYPE": "0",
            "CREATE_TIME": "2026-01-14T15:01:14+02:00",
            "UPDATE_TIME": "2026-01-14T15:01:14+02:00",
            "DELETE_TIME": null,
            "CREATED_BY": "1269",
            "UPDATED_BY": "1269",
            "DELETED_BY": "0",
            "DETAIL_URL": "https://test.bitrix24.com/company/personal/user/1269/disk/path/Folder in folder"
        },
        {
            "ID": "8907",
            "NAME": "Folder",
            "CODE": null,
            "STORAGE_ID": "1357",
            "TYPE": "folder",
            "REAL_OBJECT_ID": "8907",
            "PARENT_ID": "8875",
            "DELETED_TYPE": "0",
            "CREATE_TIME": "2025-12-30T14:16:49+02:00",
            "UPDATE_TIME": "2026-01-21T13:53:51+02:00",
            "DELETE_TIME": null,
            "CREATED_BY": "1269",
            "UPDATED_BY": "1269",
            "DELETED_BY": "0",
            "DETAIL_URL": "https://test.bitrix24.com/company/personal/user/1269/disk/path/Folder"
        },
        {
            "ID": "9023",
            "NAME": "New folder",
            "CODE": null,
            "STORAGE_ID": "1357",
            "TYPE": "folder",
            "REAL_OBJECT_ID": "9023",
            "PARENT_ID": "8875",
            "DELETED_TYPE": "0",
            "CREATE_TIME": "2026-01-26T13:30:15+02:00",
            "UPDATE_TIME": "2026-01-26T13:30:15+02:00",
            "DELETE_TIME": null,
            "CREATED_BY": "1269",
            "UPDATED_BY": "1269",
            "DELETED_BY": null,
            "DETAIL_URL": "https://test.bitrix24.com/company/personal/user/1269/disk/path/New folder"
        }
    ],
    "total": 3,
    "time": {
        "start": 1769539624,
        "finish": 1769539624.498846,
        "duration": 0.49884605407714844,
        "processing": 0,
        "date_start": "2026-01-26T14:47:04+02:00",
        "date_finish": "2026-01-26T14:47:04+02:00",
        "operating_reset_at": 1769540224,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | List of files and folders with field descriptions.

An empty array means that the user does not have permission to view the files and folders located in the root of the storage ||
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
[`datetime`](../../data-types.md) | Date and time of file/folder creation ||
|| **UPDATE_TIME**
[`datetime`](../../data-types.md) | Date and time of the last update of the file/folder ||
|| **DELETE_TIME**
[`datetime`](../../data-types.md) | Date and time the file/folder was moved to the trash ||
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
    "error_description":"Invalid value of parameter {Parameter #0}"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_ARGUMENT` | Invalid value of parameter {Parameter #0} | The required parameter `id` is not specified ||
|| `ERROR_NOT_FOUND` | Could not find entity with id `X` | The storage with the specified `id` was not found ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./disk-storage-add-folder.md)
- [{#T}](./disk-storage-get-fields.md)
- [{#T}](./disk-storage-get-for-app.md)
- [{#T}](./disk-storage-get-list.md)
- [{#T}](./disk-storage-get-types.md)
- [{#T}](./disk-storage-get.md)
- [{#T}](./disk-storage-rename.md)
- [{#T}](./disk-storage-upload-file.md)