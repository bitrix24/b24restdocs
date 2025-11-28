# Attach Files to a Task tasks.task.files.attach

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: task Creator or a user with access permission to edit the task

The method `tasks.task.files.attach` adds a file from Drive to a task. The user must have read access to the file or higher.

{% note info "" %}

Since version `tasks 25.700.0`, the method call is available in two API versions.

Old version:

`https://{installation_address}/rest/{user_id}/{rest_application_password}/tasks.task.files.attach`

New version:

`https://{installation_address}/rest/api/{user_id}/{rest_application_password}/tasks.task.file.attach`

Documentation in OpenApi format is available for the new version of the method call. To obtain OpenApi, call the method `documentation`:

`https://{installation_address}/rest/api/{user_id}/{rest_application_password}/documentation`

{% endnote %}

## Method Parameters

{% include [Note on parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **taskId***
[`integer`](../data-types.md) | The identifier of the task to which the file needs to be attached.

The task identifier can be obtained when [creating a new task](./tasks-task-add.md) or by using the [get task list method](./tasks-task-list.md).
||
|| **fileId***
[`integer`](../data-types.md) | The identifier of the file on Drive.

The file identifier can be obtained in two ways.

Use one of the file upload methods:
  - [disk.storage.uploadfile](../disk/storage/disk-storage-upload-file.md)
  - [disk.folder.uploadfile](../disk/folder/disk-folder-upload-file.md)

Use one of the methods to get the list of files:
  - [disk.storage.getchildren](../disk/storage/disk-storage-get-children.md)
  - [disk.folder.getchildren ](../disk/folder/disk-folder-get-children.md) ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskId":8017,"fileId":1065}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.files.attach
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskId":8017,"fileId":1065,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/tasks.task.files.attach
    ```

- JS

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'tasks.task.files.attach',
            {
                taskId: 8017,
                fileId: 1065
            }
        );
        
        const result = response.getData().result;
        console.log('File attached:', result);
        
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
                'tasks.task.files.attach',
                [
                    'taskId' => 8017,
                    'fileId' => 1065
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error attaching file: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.task.files.attach',
        {
            taskId: 8017,
            fileId: 1065
        },
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.task.files.attach',
        [
            'taskId' => 8017,
            'fileId' => 1065
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
        "attachmentId": 1079
    },
    "time": {
        "start": 1758806783,
        "finish": 1758806783.609955,
        "duration": 0.6099550724029541,
        "processing": 0,
        "date_start": "2025-09-25T16:26:23+02:00",
        "date_finish": "2025-09-25T16:26:23+02:00",
        "operating_reset_at": 1758807383,
        "operating": 0.4156019687652588
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | The root element of the response. Contains an object with the description of the attached file ||
|| **attachmentId**
[`integer`](../data-types.md) | The identifier of the file attachment to the task.

Data about the file can be obtained by the attachment identifier using the [disk.attachedObject.get](../disk/attached-object/disk-attached-object-get.md) method ||
|| **time**
[`time`](../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "100",
    "error_description": "Could not find value for parameter {fileId} (internal error)"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `100` | CTaskItem All parameters in the constructor must have real class type (internal error) | The required parameter `taskId` is missing ||
|| `0` | wrong task id (internal error) | The `taskId` parameter has an invalid type ||
|| `100` | Could not find value for parameter \{fileId\} (internal error) | The required parameter `fileId` is missing ||
|| `100` | Invalid value {value} to match with parameter \{fileId\}. Should be value of type int. (internal error) | The `fileId` parameter has an invalid type ||
|| `ERROR_CORE` | Insufficient permissions.\\u003Cbr\\u003E | No access to the specified file ||
|| `0` | Access denied (internal error) | Insufficient permissions to modify the task ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](../../tutorials/tasks/how-to-upload-file-to-task.md)
- [{#T}](./tasks-task-get.md)