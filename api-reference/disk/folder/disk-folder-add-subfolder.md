# Create Subfolder disk.folder.addsubfolder

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Add" or "Edit" access permission for the required folder

The method `disk.folder.addsubfolder` creates a subfolder.

The folder will inherit the same access permissions as its parent folder.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the parent folder.

The identifier can be obtained using the method [disk.storage.getchildren](../storage/disk-storage-get-children.md) if the folder is in the root of the storage, and using the method [disk.folder.getchildren](./disk-folder-get-children.md) if the folder is in another folder
||
|| **data***
[`array`](../../data-types.md) | Array with the field `NAME`, where `NAME` is the name of the subfolder ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8907,"data":{"NAME":"Folder in Folder"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/disk.folder.addsubfolder
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8907,"data":{"NAME":"Folder in Folder"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/disk.folder.addsubfolder
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'disk.folder.addsubfolder',
            {
                id: 8907,
                data: {
                    NAME: 'Folder in Folder',
                }
            }
        );
        
        const result = response.getData().result;
        console.log('Created subfolder with ID:', result);
        
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
                'disk.folder.addsubfolder',
                [
                    'id' => 8907,
                    'data' => [
                        'NAME' => 'Folder in Folder'
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
        echo 'Error adding subfolder: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "disk.folder.addsubfolder",
        {
            id: 8907,
            data: {
                NAME: 'Folder in Folder'
            },
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
        'disk.folder.addsubfolder',
        [
            'id' => 8907,
            'data' => [
                'NAME' => 'Folder in Folder'
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
        "ID": 8930,
        "NAME": "Folder in Folder",
        "CODE": null,
        "STORAGE_ID": "1357",
        "TYPE": "folder",
        "REAL_OBJECT_ID": 8930,
        "PARENT_ID": "8907",
        "DELETED_TYPE": 0,
        "CREATE_TIME": "2026-01-13T11:20:40+02:00",
        "UPDATE_TIME": "2026-01-13T11:20:40+02:00",
        "DELETE_TIME": null,
        "CREATED_BY": "1269",
        "UPDATED_BY": "1269",
        "DELETED_BY": null,
        "DETAIL_URL": "https://test.bitrix24.com/company/personal/user/1269/disk/path/Folder/Folder in Folder"
    },
    "time": {
        "start": 1768292440,
        "finish": 1768292440.894889,
        "duration": 0.8948891162872314,
        "processing": 0,
        "date_start": "2026-01-13T11:20:40+02:00",
        "date_finish": "2026-01-13T11:20:40+02:00",
        "operating_reset_at": 1768293040,
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
|| `ERROR_ARGUMENT` | Invalid value of parameter {Parameter #1} | The required field `NAME` is missing in the `data` array ||
|| `DISK_OBJ_22000` | A folder with this name already exists | A folder with this name already exists ||
|| `ERROR_NOT_FOUND` | Could not find entity with id `X` | The folder with the specified `id` was not found ||
|| `ACCESS_DENIED` | Access denied | Insufficient permissions to create the folder ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./disk-folder-copy-to.md)
- [{#T}](./disk-folder-delete-tree.md)
- [{#T}](./disk-folder-get-children.md)
- [{#T}](./disk-folder-get-external-link.md)
- [{#T}](./disk-folder-get-fields.md)
- [{#T}](./disk-folder-get.md)
- [{#T}](./disk-folder-mark-deleted.md)
- [{#T}](./disk-folder-move-to.md)
- [{#T}](./disk-folder-rename.md)
- [{#T}](./disk-folder-restore.md)
- [{#T}](./disk-folder-upload-file.md)