# Update Task tasks.task.update

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: task Creator or a user with access permission to edit the task

The method `tasks.task.update` updates a task.

## Method Parameters

{% include [Footnote about parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md)  | Task identifier.

The task identifier can be obtained when [creating a new task](./tasks-task-add.md) or by the old method of [getting the list of tasks](../../tasks/tasks-task-list.md) ||
|| **fields***
[`object`](../../data-types.md) | Values of the task fields to be updated.
[Description of all task fields](./fields.md) ||
|#

## Code Examples

{% include [Footnote about examples](../../../_includes/examples.md) %}

{% note info "" %}

The call to the new API differs by adding the parameter `/api/` in the request:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.task.update`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":11,"fields":{"title":"Task Title","deadline":"2025-12-31T23:59:59+02:00","creatorId":29,"responsibleId":1,"crmItemIds":["L_1000959"]}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":11,"fields":{"title":"Task Title","deadline":"2025-12-31T23:59:59+02:00","creatorId":29,"responsibleId":1,"crmItemIds":["L_1000959"]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.task.update
    ```

- JS

    The SDK does not yet support calls to the address /rest/api/. Use direct HTTP requests, for example, via curl or fetch.

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'tasks.task.update',
            {
                id: 11,
                fields: {
                    title: 'Task Title',
                    deadline: '2025-12-31T23:59:59+02:00',
                    creatorId: 29,
                    responsibleId: 1,
                    crmItemIds: ['L_1000959']
                }
            }
        );
        
        const result = response.getData().result;
        console.info('Task updated:', result);
    }
    catch( error )
    {
        console.error(error);
    }
    ```

- PHP

    The SDK does not yet support calls to the address /rest/api/. Use direct HTTP requests, for example, via curl or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.task.update',
                [
                    'id' => 11,
                    'fields' => [
                        'title' => 'Task Title',
                        'deadline' => '2025-12-31T23:59:59+02:00',
                        'creatorId' => 29,
                        'responsibleId' => 1,
                        'crmItemIds' => ['L_1000959'],
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating task: ' . $e->getMessage();
    }
    ```

- BX24.js

    The SDK does not yet support calls to the address /rest/api/. Use direct HTTP requests, for example, via curl or fetch.

    ```js
    BX24.callMethod(
        'tasks.task.update',
        {
            id: 11,
            fields: {
                title: 'Task Title',
                deadline: '2025-12-31T23:59:59+02:00',
                creatorId: 29,
                responsibleId: 1,
                crmItemIds: ['L_1000959']
            }
        },
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    The SDK does not yet support calls to the address /rest/api/. Use direct HTTP requests, for example, via curl or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.task.update',
        [
            'id' => 11,
            'fields' => [
                'title' => 'Task Title',
                'deadline' => '2025-12-31T23:59:59+02:00',
                'creatorId' => 29,
                'responsibleId' => 1,
                'crmItemIds' => ['L_1000959']
            ]
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
        "result": true
    },
    "time": {
        "start": 1765452441,
        "finish": 1765452442.17818,
        "duration": 1.1781799793243408,
        "processing": 1,
        "date_start": "2025-12-11T14:27:21+03:00",
        "date_finish": "2025-12-11T14:27:22+03:00",
        "operating_reset_at": 1765453041,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Root element of the response.

Contains an object with the key `result` and the value `true` if the task was successfully updated ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error during request object validation",
        "validation": [
            {
                "message": "In DTO `TaskDto`, the field `creatorId` requires the presence of the `Editable` attribute for such a request",
                "field": "creatorId"
            }
        ]
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors  

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `id`
`fields` | Required field `#FIELD#` is missing | Add the specified field to the request body ||
|| `#FIELD#` | The field `#FIELD#` requires data type `#TYPE#` for such a request | Ensure that the value being passed is of the correct type ||
|| `responsibleId` | The user specified in the "Responsible" field was not found | Specify the identifier of an existing user in the `responsibleId` field ||
|| `parentId` | The task specified in the "Parent Task" field was not found | Specify the identifier of an existing task in the `parentId` field ||
|| `parentId` | Cannot bind a node to itself | Specify a task identifier in the `parentId` field that is different from the `id` field ||
|| `endPlan` | The end date specified in the planning is earlier than the start date | Specify an `endPlan` date that is later than `startPlan` ||
|| `endPlan` | The duration of the task specified in the planning is too long | Reduce the date in the `endPlan` field ||
|| `creatorId` | In DTO `TaskDto`, the field `creatorId` requires the presence of the `Editable` attribute for such a request | Remove the `creatorId` field from the request, it cannot be changed ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `id` | Access denied | No access to the task or the task does not exist ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-task-access-get.md)
- [{#T}](./tasks-task-chat-message-send.md)
- [{#T}](./tasks-task-delete.md)