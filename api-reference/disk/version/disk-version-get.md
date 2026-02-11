# Get File Version disk.version.get

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Read" access permission for the required file

The method `disk.version.get` returns the version of a file.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id*** 
[`integer`](../../data-types.md) | Identifier of the file version.

The identifier can be obtained using the method [disk.file.getVersions](../file/disk-file-get-versions.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":7169}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/disk.version.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":7169,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/disk.version.get
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'disk.version.get',
            {
                id: 7169,
            }
        );
        
        const result = response.getData().result;
        console.log('Fetched data:', result);
        
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
                'disk.version.get',
                [
                    'id' => 7169
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching disk version: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'disk.version.get',
        {
            id: 7169
        },
        function (result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'disk.version.get',
        [
            'id' => 7169
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
        "ID": "7169",
        "OBJECT_ID": "8903",
        "SIZE": "52486",
        "NAME": "Picture.png",
        "GLOBAL_CONTENT_VERSION": "1",
        "CREATE_TIME": "2025-12-23T10:30:01+01:00",
        "CREATED_BY": "1269",
        "DOWNLOAD_URL": "https://test.bitrix24.com/rest/download.json?auth=b79c4a690000071b006e2cf2000004f50000076312db694aabed7ccf9e3d4fd1b992e3&token=disk%7CaWQ9NzE2OSZzZXJ2aWNlPXZlcnNpb24mXz0xR2VaZDdER2RBZW1WbU8xc0swSGRxYzVBclhJU0pYcQ%3D%3D%7CImRvd25sb2FkfGRpc2t8YVdROU56RTJPU1p6WlhKMmFXTmxQWFpsY25OcGIyNG1YejB4UjJWYVpEZEVSMlJCWlcxV2JVOHhjMHN3U0dSeFl6VkJjbGhKVTBwWWNRPT18Yjc5YzRhNjkwMDAwMDcxYjAwNmUyY2YyMDAwMDA0ZjUwMDAwMDc2MzEyZGI2OTRhYWJlZDdjY2Y5ZTNkNGZkMWI5OTJlMyI%3D.MCpHhZb32NO7IClkN44tbm%2FkyPM%2F4ZJ1P2uaMEPo0As%3D"
    },
    "time": {
        "start": 1766496433,
        "finish": 1766496433.618999,
        "duration": 0.6189990043640137,
        "processing": 0,
        "date_start": "2025-12-23T14:27:13+01:00",
        "date_finish": "2025-12-23T14:27:13+01:00",
        "operating_reset_at": 1766497033,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Array with version data ||
|| **ID**
[`integer`](../../data-types.md) | Identifier of the version ||
|| **OBJECT_ID**
[`integer`](../../data-types.md) | Identifier of the file to which the version belongs ||
|| **SIZE**
[`integer`](../../data-types.md) | Size of the version in bytes ||
|| **NAME**
[`string`](../../data-types.md) | Name of the file at the time the version was created ||
|| **GLOBAL_CONTENT_VERSION**
[`integer`](../../data-types.md) | Incremental version counter of the file ||
|| **CREATE_TIME**
[`string`](../../data-types.md) | Creation time of the version ||
|| **CREATED_BY**
[`integer`](../../data-types.md) | Identifier of the user who created the version ||
|| **DOWNLOAD_URL**
[`string`](../../data-types.md) | Link to download the version ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"ERROR_ARGUMENT",
    "error_description":"Invalid value of parameter `id`"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_ARGUMENT` | Invalid value of parameter `id` | Parameter `id` is missing or has an invalid type ||
|| `ERROR_NOT_FOUND` | Could not find entity with id `X` | Version with the specified `id` was not found ||
|| `ACCESS_DENIED` | Access denied | Insufficient rights to read the file ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](../../../tutorials/tasks/how-to-create-comment-with-file.md)
- [{#T}](../../../tutorials/tasks/how-to-create-task-with-file.md)
- [{#T}](../../../tutorials/tasks/how-to-upload-file-to-task.md)