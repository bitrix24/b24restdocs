# Get Public Link for File disk.file.getExternalLink

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: user with "Read" permission for the required file

The method `disk.file.getExternalLink` returns a public link to a file.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the file.

The identifier can be obtained using the method [disk.storage.getchildren](../storage/disk-storage-get-children.md) if the file is in the root of the storage, and using the method [disk.folder.getchildren](../folder/disk-folder-get-children.md) if the file is in a folder ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8964}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/disk.file.getExternalLink
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8964,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/disk.file.getExternalLink
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'disk.file.getExternalLink',
            {
                id: 8964,
            }
        );
        
        const result = response.getData().result;
        console.log('External link:', result);
        
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
                'disk.file.getExternalLink',
                [
                    'id' => 8964
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting external link: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "disk.file.getExternalLink",
        {
            id: 8964
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
        'disk.file.getExternalLink',
        [
            'id' => 8964
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
    "result": "https://test.bitrix24.com/~4Pie9",
    "time": {
        "start": 1770651076,
        "finish": 1770651076.172321,
        "duration": 0.17232108116149902,
        "processing": 0,
        "date_start": "2026-02-09T15:31:16+01:00",
        "date_finish": "2026-02-09T15:31:16+01:00",
        "operating_reset_at": 1770651676,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`string`](../../data-types.md) | Public link to the file ||
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
|| `ERROR_ARGUMENT` | Invalid value of parameter {Parameter #0} | Required parameter `id` is missing ||
|| `ERROR_NOT_FOUND` | Could not find entity with id `X` | File with the specified `id` not found ||
|| `ACCESS_DENIED` | Access denied | Insufficient permissions to obtain the file link ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./disk-file-copy-to.md)
- [{#T}](./disk-file-delete.md)
- [{#T}](./disk-file-get-fields.md)
- [{#T}](./disk-file-get-versions.md)
- [{#T}](./disk-file-get.md)
- [{#T}](./disk-file-mark-deleted.md)
- [{#T}](./disk-file-move-to.md)
- [{#T}](./disk-file-rename.md)
- [{#T}](./disk-file-restore-from-version.md)
- [{#T}](./disk-file-restore.md)
- [{#T}](./disk-file-upload-version.md)