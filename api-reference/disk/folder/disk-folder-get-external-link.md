# Get Public Link for Folder disk.folder.getexternallink

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: user with "Read" access permission for the required folder

The method `disk.folder.getexternallink` returns a public link to the folder.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the folder.

The identifier can be obtained using the method [disk.storage.getchildren](../storage/disk-storage-get-children.md) if the folder is in the root of the storage, and using the method [disk.folder.getchildren](./disk-folder-get-children.md) if the folder is in another folder ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8930}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/disk.folder.getexternallink
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8930,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/disk.folder.getexternallink
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'disk.folder.getexternallink',
            {
                id: 8930
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
                'disk.folder.getexternallink',
                [
                    'id' => 8930
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
        "disk.folder.getexternallink",
        {
            id: 8930
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
        'disk.folder.getexternallink',
        [
            'id' => 8930
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
    "result": "https://test.bitrix24.com/~Co13P",
    "time": {
        "start": 1768397792,
        "finish": 1768397792.968311,
        "duration": 0.968311071395874,
        "processing": 0,
        "date_start": "2026-01-14T15:36:32+02:00",
        "date_finish": "2026-01-14T15:36:32+02:00",
        "operating_reset_at": 1768398392,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`string`](../../data-types.md) | Public link to the folder ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
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
|| `ERROR_NOT_FOUND` | Could not find entity with id `X` | Folder with the specified `id` was not found ||
|| `ACCESS_DENIED` | Access denied | Insufficient permissions to obtain the link to the folder ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./disk-folder-add-subfolder.md)
- [{#T}](./disk-folder-copy-to.md)
- [{#T}](./disk-folder-delete-tree.md)
- [{#T}](./disk-folder-get-children.md)
- [{#T}](./disk-folder-get-fields.md)
- [{#T}](./disk-folder-get.md)
- [{#T}](./disk-folder-mark-deleted.md)
- [{#T}](./disk-folder-move-to.md)
- [{#T}](./disk-folder-rename.md)
- [{#T}](./disk-folder-restore.md)
- [{#T}](./disk-folder-share-to-user.md)
- [{#T}](./disk-folder-upload-file.md)