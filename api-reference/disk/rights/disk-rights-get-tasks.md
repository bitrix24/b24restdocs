# Get a List of Available Access Levels disk.rights.getTasks

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.rights.getTasks` returns a list of available access levels.

Use the obtained access level identifiers to set permissions on files during their upload. Specify the identifiers as the value of the `TASK_ID` parameter in the methods [disk.storage.uploadfile](../storage/disk-storage-upload-file.md) and [disk.folder.uploadfile](../folder/disk-folder-upload-file.md).

## Method Parameters

No parameters.

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/disk.rights.getTasks
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/disk.rights.getTasks
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'disk.rights.getTasks',
            {}
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
                'disk.rights.getTasks',
                []
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
        'disk.rights.getTasks',
        {},
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
        'disk.rights.getTasks',
        []
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
        "ID": "79",
        "NAME": "disk_access_full",
        "TITLE": "Full Access"
        },
        {
        "ID": "75",
        "NAME": "disk_access_edit",
        "TITLE": "Editing"
        },
        {
        "ID": "71",
        "NAME": "disk_access_read",
        "TITLE": "Reading"
        }
    ],
    "time": {
        "start": 1766494790,
        "finish": 1766494790.095506,
        "duration": 0.09550595283508301,
        "processing": 0,
        "date_start": "2025-12-23T12:59:50+01:00",
        "date_finish": "2025-12-23T12:59:50+01:00",
        "operating_reset_at": 1766495390,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Array of available access levels ||
|| **ID**
[`integer`](../../data-types.md) | Access level identifier ||
|| **NAME**
[`string`](../../data-types.md) | Symbolic code of the access level ||
|| **TITLE**
[`string`](../../data-types.md) | Name of the access level ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](../../../tutorials/tasks/how-to-create-comment-with-file.md)
- [{#T}](../../../tutorials/tasks/how-to-create-task-with-file.md)
- [{#T}](../../../tutorials/tasks/how-to-upload-file-to-task.md)