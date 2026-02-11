# Permanently Delete File disk.file.delete

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Full access" permission for the specified file

The method `disk.file.delete` permanently deletes a file.

If you want to move the file to the trash, use the method [disk.file.markdeleted](./disk-file-mark-deleted.md).

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the file.

The identifier can be obtained using the method [disk.storage.getchildren](../storage/disk-storage-get-children.md) if the file is in the root of the storage, and using the method [disk.folder.getchildren](../folder/disk-folder-get-children.md) if the file is in a folder
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
    -d '{"id":9035}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/disk.file.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":9035,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/disk.file.delete
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'disk.file.delete',
            {
                id: 9035,
            }
        );
        
        const result = response.getData().result;
        console.log('Deleted file with ID:', result);
        
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
                'disk.file.delete',
                [
                    'id' => 9035
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting file: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "disk.file.delete",
        {
            id: 9035
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
        'disk.file.delete',
        [
            'id' => 9035
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
        "start": 1770649732,
        "finish": 1770649732.50942,
        "duration": 0.5094199180603027,
        "processing": 0,
        "date_start": "2026-02-09T13:08:52+01:00",
        "date_finish": "2026-02-09T13:08:52+01:00",
        "operating_reset_at": 1770650332,
        "operating": 0.2615058422088623
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if the file was successfully deleted ||
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
|| `DISK_FILE_22007` | Exclusive lock | The file is open for editing by another user ||
|| `ACCESS_DENIED` | Access denied | Insufficient permissions to delete the file ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./disk-file-copy-to.md)
- [{#T}](./disk-file-get-external-link.md)
- [{#T}](./disk-file-get-fields.md)
- [{#T}](./disk-file-get-versions.md)
- [{#T}](./disk-file-get.md)
- [{#T}](./disk-file-mark-deleted.md)
- [{#T}](./disk-file-move-to.md)
- [{#T}](./disk-file-rename.md)
- [{#T}](./disk-file-restore-from-version.md)
- [{#T}](./disk-file-restore.md)
- [{#T}](./disk-file-upload-version.md)