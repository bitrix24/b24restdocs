# Move Folder to Trash disk.folder.markdeleted

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Full access" permission for the required folder

The method `disk.folder.markdeleted` moves a folder to the trash.

{% note info "" %}

Save the folder ID after deletion so that it can be restored later using the method [disk.folder.restore](./disk-folder-restore.md)

{% endnote %} 

## Method Parameters

{% include [Footnote about parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the folder.

The identifier can be obtained using the method [disk.folder.getchildren](./disk-folder-get-children.md) ||
|#

{% note info "" %}

You cannot move the root folder of the storage to the trash.

{% endnote %} 

## Code Examples

{% include [Footnote about examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8996}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/disk.folder.markdeleted
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8996,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/disk.folder.markdeleted
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'disk.folder.markdeleted',
            {
                id: 8996,
            }
        );
        
        const result = response.getData().result;
        console.log('Folder marked as deleted with ID:', result);
        
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
                'disk.folder.markdeleted',
                [
                    'id' => 8996
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error marking folder as deleted: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "disk.folder.markdeleted",
        {
            id: 8996
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
        'disk.folder.markdeleted',
        [
            'id' => 8996
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
        "ID": "8996",
        "NAME": "Folder",
        "CODE": null,
        "STORAGE_ID": "1357",
        "TYPE": "folder",
        "REAL_OBJECT_ID": "8996",
        "PARENT_ID": "8907",
        "DELETED_TYPE": 3,
        "CREATE_TIME": "2026-01-21T13:53:51+02:00",
        "UPDATE_TIME": "2026-01-21T13:53:51+02:00",
        "DELETE_TIME": "2026-01-21T13:56:54+02:00",
        "CREATED_BY": "1269",
        "UPDATED_BY": "1269",
        "DELETED_BY": "1269",
        "DETAIL_URL": "https://test.bitrix24.com/company/personal/user/1269/disk/path/Folder/Folder"
    },
    "time": {
        "start": 1768993014,
        "finish": 1768993014.106899,
        "duration": 0.10689902305603027,
        "processing": 0,
        "date_start": "2026-01-21T13:56:54+02:00",
        "date_finish": "2026-01-21T13:56:54+02:00",
        "operating_reset_at": 1768993614,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Array with data about the moved folder in the trash ||
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
- `3` — in trash
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

HTTP Status: **400**

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
|| `ERROR_ARGUMENT` | Invalid value of parameter {Parameter #1} | Required parameter `id` is missing ||
|| `DISK_OBJ_22000` | Could not delete root folder | Attempt to move the root folder of the storage to the trash ||
|| `ERROR_NOT_FOUND` | Could not find entity with id `X` | Folder with the specified `id` not found ||
|| `ACCESS_DENIED` | Access denied | Insufficient permissions to move the folder to the trash ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./disk-folder-add-subfolder.md)
- [{#T}](./disk-folder-copy-to.md)
- [{#T}](./disk-folder-delete-tree.md)
- [{#T}](./disk-folder-get-children.md)
- [{#T}](./disk-folder-get-external-link.md)
- [{#T}](./disk-folder-get-fields.md)
- [{#T}](./disk-folder-get.md)
- [{#T}](./disk-folder-move-to.md)
- [{#T}](./disk-folder-rename.md)
- [{#T}](./disk-folder-restore.md)
- [{#T}](./disk-folder-share-to-user.md)
- [{#T}](./disk-folder-upload-file.md)