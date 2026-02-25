# List of File Versions for disk.file.getVersions

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Read" access permission for the specified file

The method `disk.file.getVersions` returns a list of file versions.

Versions are returned in descending order of creation date.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the file.

The identifier can be obtained using the method [disk.storage.getchildren](../storage/disk-storage-get-children.md) if the file is located at the root of the storage, and using the method [disk.folder.getchildren](../folder/disk-folder-get-children.md) if the file is located in a folder ||
|| **filter**
[`array`](../../data-types.md) | An array in the following format:

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
- `%` — LIKE, substring search. The `%` symbol should not be included in the filter value. The search looks for the substring at any position in the string
- `=%` — LIKE, substring search. The `%` symbol should be included in the value. Examples:
    - `"test%"` — searches for values starting with "test"
    - `"%test"` — searches for values ending with "test"
    - `"%test%"` — searches for values where "test" can be at any position
- `%=` — LIKE (similar to `=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal

Available fields for filtering:

- `ID` — version identifier
- `SIZE` — size of the version in bytes
- `NAME` — name of the file at the time the version was created
- `CREATE_TIME` — time of version creation
- `CREATED_BY` — identifier of the user who created the version
||
|| **start**
[`integer`](../../data-types.md) | This parameter is used for managing pagination.

The page size for results is always static — 50 records.

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
    -d '{"id":9043,"filter":{"NAME":"%test%"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/disk.file.getVersions
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":9043,"filter":{"NAME":"%test%"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/disk.file.getVersions
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'disk.file.getVersions',
            {
                id: 9043,
                filter: {
                    NAME: '%test%',
                }
            }
        );
        
        const result = response.getData().result;
        console.log('Fetched file versions:', result);
        
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
                'disk.file.getVersions',
                [
                    'id' => 9043,
                    'filter' => [
                        'NAME' => '%test%'
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
        echo 'Error fetching file versions: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "disk.file.getVersions",
        {
            id: 9043,
            filter: {
                NAME: '%test%'
            },
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
        'disk.file.getVersions',
        [
            'id' => 9043,
            'filter' => [
                'NAME' => '%test%'
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
    "result": [
        {
        "ID": "7201",
        "OBJECT_ID": "9043",
        "SIZE": "21796",
        "NAME": "Test №2.docx",
        "GLOBAL_CONTENT_VERSION": "5",
        "CREATE_TIME": "2026-02-17T12:21:22+02:00",
        "CREATED_BY": "1271",
        "DOWNLOAD_URL": "https://test.bitrix24.com/rest/download.json?auth=343794690000071b006e2cf2000004f500000746484b1f82771b3434ff80eb15edc8f8&token=disk%7CaWQ9NzIwMSZzZXJ2aWNlPXZlcnNpb24mXz0zdHFiVHY5bkNPWm5hVk1rRm92TXlzZUdUSDNTanlVZQ%3D%3D%7CImRvd25sb2FkfGRpc2t8YVdROU56SXdNU1p6WlhKMmFXTmxQWFpsY25OcGIyNG1YejB6ZEhGaVZIWTVia05QV201aFZrMXJSbTkyVFhselpVZFVTRE5UYW5sVlpRPT18MzQzNzk0NjkwMDAwMDcxYjAwNmUyY2YyMDAwMDA0ZjUwMDAwMDc0NjQ4NGIxZjgyNzcxYjM0MzRmZjgwZWIxNWVkYzhmOCI%3D.dy270nYUZXvxmyBAR1vYnUtn%2Bkn%2FSClb2cIWT8FkOn0%3D"
        },
        {
        "ID": "7199",
        "OBJECT_ID": "9043",
        "SIZE": "21756",
        "NAME": "test.docx",
        "GLOBAL_CONTENT_VERSION": "3",
        "CREATE_TIME": "2026-02-17T12:15:06+02:00",
        "CREATED_BY": "1269",
        "DOWNLOAD_URL": "https://test.bitrix24.com/rest/download.json?auth=343794690000071b006e2cf2000004f500000746484b1f82771b3434ff80eb15edc8f8&token=disk%7CaWQ9NzE5OSZzZXJ2aWNlPXZlcnNpb24mXz1Uall3RkZRa0NPMTBncklPM2tYYmxNajRmSWQ2ekVLNg%3D%3D%7CImRvd25sb2FkfGRpc2t8YVdROU56RTVPU1p6WlhKMmFXTmxQWFpsY25OcGIyNG1YejFVYWxsM1JrWlJhME5QTVRCbmNrbFBNMnRZWW14TmFqUm1TV1EyZWtWTE5nPT18MzQzNzk0NjkwMDAwMDcxYjAwNmUyY2YyMDAwMDA0ZjUwMDAwMDc0NjQ4NGIxZjgyNzcxYjM0MzRmZjgwZWIxNWVkYzhmOCI%3D.mtFVuU%2F1h4eGP1VROTj7n4PDUDOSc4suh90NuNPQyyQ%3D"
        }
    ],
    "total": 2,
    "time": {
        "start": 1771320110,
        "finish": 1771320110.716905,
        "duration": 0.7169051170349121,
        "processing": 0,
        "date_start": "2026-02-17T12:21:50+02:00",
        "date_finish": "2026-02-17T12:21:50+02:00",
        "operating_reset_at": 1771320710,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | List of file versions.

An empty array means there are no versions that meet the filter criteria ||
|| **ID**
[`integer`](../../data-types.md) | Version identifier ||
|| **OBJECT_ID**
[`integer`](../../data-types.md) | Identifier of the file to which the version belongs ||
|| **SIZE**
[`integer`](../../data-types.md) | Size of the version in bytes ||
|| **NAME**
[`string`](../../data-types.md) | Name of the file at the time the version was created ||
|| **GLOBAL_CONTENT_VERSION**
[`integer`](../../data-types.md) | Incremental version counter for the file ||
|| **CREATE_TIME**
[`string`](../../data-types.md) | Time of version creation ||
|| **CREATED_BY**
[`integer`](../../data-types.md) | Identifier of the user who created the version ||
|| **DOWNLOAD_URL**
[`string`](../../data-types.md) | Link to download the version ||
|| **total**
[`integer`](../../data-types.md) | Total number of file versions ||
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
|| `ERROR_ARGUMENT` | Invalid value of parameter {Parameter #0} | The required parameter `id` is missing ||
|| `ERROR_NOT_FOUND` | Could not find entity with id `X` | The file with the specified `id` was not found ||
|| `ACCESS_DENIED` | Access denied | Insufficient permissions to retrieve file versions ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./disk-file-copy-to.md)
- [{#T}](./disk-file-delete.md)
- [{#T}](./disk-file-get-external-link.md)
- [{#T}](./disk-file-get-fields.md)
- [{#T}](./disk-file-get.md)
- [{#T}](./disk-file-mark-deleted.md)
- [{#T}](./disk-file-move-to.md)
- [{#T}](./disk-file-rename.md)
- [{#T}](./disk-file-restore-from-version.md)
- [{#T}](./disk-file-restore.md)
- [{#T}](./disk-file-upload-version.md)