# Get the list of task results tasks.task.result.list

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user with access to the task

The method `tasks.task.result.list` retrieves the list of results associated with a task.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **taskId***
[`integer`](../../data-types.md) | The identifier of the task from which to retrieve results.

The task identifier can be obtained when [creating a new task](../tasks-task-add.md) or by using the [get task list method](../tasks-task-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskId":8017}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.result.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskId":8017,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/tasks.task.result.list
    ```

- JS

    ```javascript
    // callListMethod is recommended when you need to retrieve
    // the entire set of list data and the volume of records is relatively small
    // (up to about 1000 items). The method loads all data at once, which
    // can lead to high memory load when working with large volumes.

    try {
    const response = await $b24.callListMethod(
        'tasks.task.result.list',
        { taskId: 8017 },
        (progress: number) => { console.log('Progress:', progress) }
    );
    const items = response.getData() || [];
    for (const entity of items) { console.log('Entity:', entity) }
    } catch (error: any) {
    console.error('Request failed', error)
    }

    // fetchListMethod is preferable when working with large datasets.
    // The method implements iterative sampling using a generator, which
    // allows processing data in parts and efficiently using memory.

    try {
    const generator = $b24.fetchListMethod('tasks.task.result.list', { taskId: 8017 }, 'ID');
    for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
    }
    } catch (error: any) {
    console.error('Request failed', error)
    }

    // callMethod provides manual control over the pagination
    // of data retrieval through the start parameter. Suitable for scenarios where
    // precise control over request batches is required. However, with large
    // volumes of data, it may be less efficient compared to
    // fetchListMethod.

    try {
    const response = await $b24.callMethod('tasks.task.result.list', { taskId: 8017 }, 0);
    const result = response.getData().result || [];
    for (const entity of result) { console.log('Entity:', entity) }
    } catch (error: any) {
    console.error('Request failed', error)
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.task.result.list',
                [
                    'taskId' => 8017
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error listing task results: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.task.result.list',
        {
            "taskId": 8017
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
        'tasks.task.result.list',
        [
            'taskId' => 8017
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
    "result": [
        {
            "id": 23,
            "taskId": 8017,
            "commentId": 3197,
            "createdBy": 503,
            "createdAt": "2025-07-15T14:30:00+02:00",
            "updatedAt": "2025-08-19T16:45:48+02:00",
            "status": 0,
            "text": "The client signed the documents",
            "formattedText": "The client signed the documents",
            "files": []
        },
        {
            "id": 21,
            "taskId": 8017,
            "commentId": 3199,
            "createdBy": 503,
            "createdAt": "2025-07-13T14:30:00+02:00",
            "updatedAt": "2025-08-19T16:45:56+02:00",
            "status": 0,
            "text": "Sent documents to the client. The client promises to respond on [B]Monday[\/B].",
            "formattedText": "Sent documents to the client. The client promises to respond on \u003Cb\u003EMonday\u003C\/b\u003E.",
            "files": [1055,1057,1059,1061,1063]
        }
    ],
    "time": {
        "start": 1755611166.509052,
        "finish": 1755611166.542696,
        "duration": 0.03364396095275879,
        "processing": 0.00906991958618164,
        "date_start": "2025-08-19T16:46:06+02:00",
        "date_finish": "2025-08-19T16:46:06+02:00",
        "operating_reset_at": 1755611766,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | An array of objects, where each object describes a task result ||
|| **id**
[`integer`](../../data-types.md) | The identifier of the result ||
|| **taskId**
[`integer`](../../data-types.md) | The identifier of the task ||
|| **commentId**
[`integer`](../../data-types.md) | The identifier of the comment marked as a result ||
|| **createdBy**
[`integer`](../../data-types.md) | The identifier of the user who marked the result ||
|| **createdAt**
[`string`](../../data-types.md) | The date and time the result was marked in ISO 8601 format ||
|| **updatedAt**
[`string`](../../data-types.md) | The date and time of the last modification of the result in ISO 8601 format ||
|| **status**
[`integer`](../../data-types.md) | The status of the result. Possible values:
- `0` — result is open
- `1` — result is closed

The result becomes closed after the task is completed and retains this status after the task is resumed. Only new results in an unfinished task will be open.

A comment with an open result cannot be added again to the result. If the result is closed, adding is possible
 ||
|| **text**
[`string`](../../data-types.md) | The text of the result ||
|| **formattedText**
[`string`](../../data-types.md) | The text of the result with formatting ||
|| **files**
[`array`](../../data-types.md) | A list of file identifiers attached to the result.

Contains an empty array if there are no files in the comment ||
|| **time**
[`time`](../../data-types.md#time) | Information about the time taken for the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"100",
    "error_description":"Invalid value {value} to match with parameter {commentId}. Should be value of type int."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | Access denied. | The user does not have access to the task or a task with such `ID` does not exist ||
|| `100` | Invalid value {value} to match with parameter {commentId}. Should be value of type int. | An invalid type value was passed in the `taskId` parameter. It should be of type `integer` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./tasks-task-result-add-from-comment.md)
- [{#T}](./tasks-task-result-delete-from-comment.md)