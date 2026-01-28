# Permanently Delete a Folder and All Its Contents disk.folder.deletetree

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Full access" permission for the specified folder

The method `disk.folder.deletetree` permanently deletes a folder and all its contents.

If you want to move the folder to the trash, use the method [disk.folder.markdeleted](./disk-folder-mark-deleted.md).

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the folder.

The identifier can be obtained using the method [disk.folder.getchildren](./disk-folder-get-children.md)
||
|#

{% note info "" %}

You cannot delete the root folder of the storage

{% endnote %} 

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8942}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/disk.folder.deletetree
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8942,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/disk.folder.deletetree
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'disk.folder.deletetree',
            {
                id: 8942,
            }
        );
        
        const result = response.getData().result;
        console.log('Deleted folder tree with ID:', result);
        
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
                'disk.folder.deletetree',
                [
                    'id' => 8942
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting folder tree: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "disk.folder.deletetree",
        {
            id: 8942
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
        'disk.folder.deletetree',
        [
            'id' => 8942
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
    "result": true,
    "time": {
        "start": 1768316913,
        "finish": 1768316913.467206,
        "duration": 0.4672060012817383,
        "processing": 0,
        "date_start": "2026-01-13T15:08:33+01:00",
        "date_finish": "2026-01-13T15:08:33+01:00",
        "operating_reset_at": 1768317513,
        "operating": 0.3324289321899414
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if the folder was successfully deleted ||
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
|| `DISK_OBJ_22000` | Could not delete root folder | Attempt to delete the root folder of the storage ||
|| `ERROR_NOT_FOUND` | Could not find entity with id `X` | Folder with the specified `id` was not found ||
|| `ACCESS_DENIED` | Access denied | Insufficient permissions to delete the folder ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./disk-folder-add-subfolder.md)
- [{#T}](./disk-folder-copy-to.md)
- [{#T}](./disk-folder-get-children.md)
- [{#T}](./disk-folder-get-external-link.md)
- [{#T}](./disk-folder-get-fields.md)
- [{#T}](./disk-folder-get.md)
- [{#T}](./disk-folder-mark-deleted.md)
- [{#T}](./disk-folder-move-to.md)
- [{#T}](./disk-folder-rename.md)
- [{#T}](./disk-folder-restore.md)
- [{#T}](./disk-folder-share-to-user.md)
- [{#T}](./disk-folder-upload-file.md)