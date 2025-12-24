# Get Information About Attached File disk.attachedObject.get

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Read" access permission for the required file

The method `disk.attachedObject.get` returns information about the attached file.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the file attachment record, which is the link connecting the disk file [with other objects](../index.md#diskconnection).

To obtain the connection identifier, use methods that return attached files. For example, if the file is attached to a task, you can find out the connection identifier using the [tasks.task.get](../../tasks/tasks-task-get.md) method.
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
    -d '{"id":495}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/disk.attachedObject.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":495,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/disk.attachedObject.get
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'disk.attachedObject.get',
            {
                id: 495,
            }
        );
        
        const result = response.getData().result;
        console.log('Retrieved object:', result);
        
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
                'disk.attachedObject.get',
                [
                    'id' => 495
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error retrieving attached object: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'disk.attachedObject.get',
        {
            id: 495
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
        'disk.attachedObject.get',
        [
            'id' => 495
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
        "ID": "495",
        "OBJECT_ID": "8903",
        "MODULE_ID": "tasks",
        "ENTITY_TYPE": "tasks_task",
        "ENTITY_ID": "3845",
        "CREATE_TIME": "2025-12-23T10:31:24+02:00",
        "CREATED_BY": "1269",
        "DOWNLOAD_URL": "https://test.bitrix24.com/bitrix/tools/disk/uf.php?attachedId=495&auth[auth]=d78a4a690000071b006e2cf2000004f5000007746b9ad166e1b9bd67b8848714afc5a7&action=download&ncc=1",
        "NAME": "Picture.png",
        "SIZE": "52486"
    },
    "time": {
        "start": 1766489404,
        "finish": 1766489404.720053,
        "duration": 0.72005295753479,
        "processing": 0,
        "date_start": "2025-12-23T11:30:04+02:00",
        "date_finish": "2025-12-23T11:30:04+02:00",
        "operating_reset_at": 1766490004,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object containing data about the attached file ||
|| **ID**
[`integer`](../../data-types.md) | Identifier of the file attachment record ||
|| **OBJECT_ID**
[`integer`](../../data-types.md) | Identifier of the file ||
|| **MODULE_ID**
[`string`](../../data-types.md) | Module where the file is used ||
|| **ENTITY_TYPE**
[`string`](../../data-types.md) | Type of the linked object ||
|| **ENTITY_ID**
[`integer`](../../data-types.md) | Identifier of the item to which the file is attached ||
|| **CREATE_TIME**
[`string`](../../data-types.md) | Time of the attachment creation ||
|| **CREATED_BY**
[`integer`](../../data-types.md) | Identifier of the user who attached the file ||
|| **DOWNLOAD_URL**
[`string`](../../data-types.md) | Link to download the file ||
|| **NAME**
[`string`](../../data-types.md) | Name of the file ||
|| **SIZE**
[`integer`](../../data-types.md) | Size of the file in bytes ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":"ERROR_NOT_FOUND",
    "error_description":"Could not find entity with id `X`"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_NOT_FOUND` | Could not find entity with id `X` | Attached file with the specified `id` not found ||
|| `ACCESS_DENIED` | Access denied | Insufficient rights to read the attached file ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](../../../tutorials/tasks/how-to-create-comment-with-file.md)
- [{#T}](../../../tutorials/tasks/how-to-create-task-with-file.md)
- [{#T}](../../../tutorials/tasks/how-to-upload-file-to-task.md)