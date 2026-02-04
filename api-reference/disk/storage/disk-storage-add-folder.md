# Create a Folder in the Root of the Storage disk.storage.addfolder

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: user with "Add" access permission for the required storage

The method `disk.storage.addfolder` creates a folder in the root of the storage.

## Method Parameters

{% include [Footnote about parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the storage.

The identifier can be obtained using the method [disk.storage.getlist](./disk-storage-get-list.md)
||
|| **data***
[`array`](../../data-types.md) | Array with the field `NAME`, where `NAME` is the name of the new folder ||
|| **rights**
[`array`](../../data-types.md) | Array of access permissions for the folder in the format `{"TASK_ID": 42, "ACCESS_CODE": "U35"}`, where
- `TASK_ID` — identifier of the access level
- `ACCESS_CODE` — access code consisting of the user's or department's letter code and identifier

User categories:
- `U` — user
- `*` — all users
- `D` — all department employees
- `DR` — all department employees with subdivisions

The list of available `TASK_ID` identifiers for setting permissions can be obtained using the method [disk.rights.getTasks](../rights/disk-rights-get-tasks.md) ||
|#

## Code Examples

{% include [Footnote about examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1357,"data":{"NAME":"New Folder"},"rights":[{"TASK_ID":71,"ACCESS_CODE":"U1271"}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/disk.storage.addfolder
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1357,"data":{"NAME":"New Folder"},"rights":[{"TASK_ID":71,"ACCESS_CODE":"U1271"}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/disk.storage.addfolder
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'disk.storage.addfolder',
            {
                id: 1357,
                data: {
                    NAME: 'New Folder',
                },
                rights: [
                    {
                        TASK_ID: 71,
                        ACCESS_CODE: 'U1271'
                    }
                ]
            }
        );
        
        const result = response.getData().result;
        console.log('Created folder with ID:', result);
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
                'disk.storage.addfolder',
                [
                    'id' => 1357,
                    'data' => [
                        'NAME' => 'New Folder'
                    ],
                    'rights' => [
                        [
                            'TASK_ID' => 71,
                            'ACCESS_CODE' => 'U1271'
                        ]
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
        echo 'Error adding folder: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "disk.storage.addfolder",
        {
            id: 1357,
            data: {
                NAME: 'New Folder'
            },
            rights: [
                {
                    TASK_ID: 71,
                    ACCESS_CODE: 'U1271'
                }
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
        'disk.storage.addfolder',
        [
            'id' => 1357,
            'data' => [
                'NAME' => 'New Folder'
            ],
            'rights' => [
                [
                    'TASK_ID' => 71,
                    'ACCESS_CODE' => 'U1271'
                ]
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
    "result": {
        "ID": 9031,
        "NAME": "New Folder",
        "CODE": null,
        "STORAGE_ID": "1357",
        "TYPE": "folder",
        "REAL_OBJECT_ID": 9031,
        "PARENT_ID": "8875",
        "DELETED_TYPE": 0,
        "CREATE_TIME": "2026-01-28T17:23:11+02:00",
        "UPDATE_TIME": "2026-01-28T17:23:11+02:00",
        "DELETE_TIME": null,
        "CREATED_BY": "1269",
        "UPDATED_BY": "1269",
        "DELETED_BY": null,
        "DETAIL_URL": "https://test.bitrix24.com/company/personal/user/1269/disk/path/New Folder"
    },
    "time": {
        "start": 1769610191,
        "finish": 1769610191.803601,
        "duration": 0.8036010265350342,
        "processing": 0,
        "date_start": "2026-01-28T17:23:11+02:00",
        "date_finish": "2026-01-28T17:23:11+02:00",
        "operating_reset_at": 1769610791,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Array with data about the created folder ||
|| **ID**
[`integer`](../../data-types.md) | Identifier of the folder ||
|| **NAME**
[`string`](../../data-types.md) | Name of the folder ||
|| **CODE**
[`string`](../../data-types.md) | Symbolic code of the folder ||
|| **STORAGE_ID**
[`integer`](../../data-types.md) | Identifier of the storage where the folder is located ||
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
|| **CREATE_TIME**
[`datetime`](../../data-types.md) | Date and time of folder creation ||
|| **UPDATE_TIME**
[`datetime`](../../data-types.md) | Date and time of the last update of the folder ||
|| **DELETE_TIME**
[`datetime`](../../data-types.md) | Date and time of moving the folder to the trash ||
|| **CREATED_BY**
[`integer`](../../data-types.md) | Identifier of the user who created the folder ||
|| **UPDATED_BY**
[`integer`](../../data-types.md) | Identifier of the user who made the last change ||
|| **DELETED_BY**
[`integer`](../../data-types.md) | Identifier of the user who deleted the folder ||
|| **DETAIL_URL**
[`string`](../../data-types.md) | Link to open the folder in the interface ||
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
|| `ERROR_ARGUMENT` | Invalid value of parameter {Parameter #1} | Required field `NAME` not provided in the `data` array ||
|| `DISK_OBJ_22000` | A folder with this name already exists | A folder with this name already exists ||
|| `ERROR_NOT_FOUND` | Could not find entity with id `X` | Storage with the specified `id` not found ||
|| `ACCESS_DENIED` | Access denied | Insufficient permissions to create the folder ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./disk-storage-get-children.md)
- [{#T}](./disk-storage-get-fields.md)
- [{#T}](./disk-storage-get-for-app.md)
- [{#T}](./disk-storage-get-list.md)
- [{#T}](./disk-storage-get-types.md)
- [{#T}](./disk-storage-get.md)
- [{#T}](./disk-storage-rename.md)
- [{#T}](./disk-storage-upload-file.md)