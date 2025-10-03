# Get Task History tasks.task.history.list

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user with read access to the task

The method `tasks.task.history.list` retrieves the history of changes for a task.

## Method Parameters

{% include [Note on parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **taskId***
[`integer`](../data-types.md) | The identifier of the task for which to retrieve the history.

The task identifier can be obtained when [creating a new task](./tasks-task-add.md) or by using the [get task list method](./tasks-task-list.md) ||
|| **filter**
[`object`](../data-types.md) | Filter by event type in the format `{FIELD: 'EVENT'}`. A list of possible values for `FIELD` is described [below](#lists)
||
|| **order**
[`object`](../data-types.md) | An object for sorting the result in the form `{"field": "sort value", ... }`.

The sorting direction can take the following values:

- `asc` — ascending
- `desc` — descending

By default, records are sorted in descending order by creation time, meaning from newest to oldest ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskId":8137,"filter":{"FIELD":"COMMENT"},"order":{"createdDate":"ASC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.history.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskId":8137,"filter":{"FIELD":"COMMENT"},"order":{"createdDate":"ASC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/tasks.task.history.list
    ```

- JS

    ```javascript
    // callListMethod: Retrieves all data at once.
    // Use only for small selections (< 1000 items) due to high
    // memory load.

    try {
    const response = await $b24.callListMethod(
        'tasks.task.history.list',
        {
            taskId: 8137,
            filter: { FIELD: 'COMMENT' },
            order: { createdDate: 'ASC' }
        },
        (progress) => { console.log('Progress:', progress) }
    );
    const items = response.getData() || [];
    for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
    console.error('Request failed', error)
    }

    // fetchListMethod: Retrieves data in parts using an iterator.
    // Use for large volumes of data for efficient memory consumption.

    try {
    const generator = $b24.fetchListMethod('tasks.task.history.list', {
        taskId: 8137,
        filter: { FIELD: 'COMMENT' },
        order: { createdDate: 'ASC' }
    }, 'ID');
    for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
    }
    } catch (error) {
    console.error('Request failed', error)
    }

    // callMethod: Manual control of pagination through the start parameter.
    // Use for precise control over request batches.
    // Less efficient for large data than fetchListMethod.

    try {
    const response = await $b24.callMethod('tasks.task.history.list', {
        taskId: 8137,
        filter: { FIELD: 'COMMENT' },
        order: { createdDate: 'ASC' }
    }, 0);
    const result = response.getData().result || [];
    for (const entity of result) { console.log('Entity:', entity) }
    } catch (error) {
    console.error('Request failed', error)
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.task.history.list',
                [
                    'taskId' => 8137,
                    'filter' => ['FIELD' => 'COMMENT'],
                    'order' => ['createdDate' => 'ASC']
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching task history: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.task.history.list',
        {
            taskId: 8137,
            filter: { FIELD: 'COMMENT' },
            order: {createdDate: 'ASC'},
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
        'tasks.task.history.list',
        [
            'taskId' => 8137,
            'filter' => ['FIELD' => 'COMMENT'],
            'order' => ['createdDate' => 'ASC']
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
        "list": [
            {
                "id": 16359,
                "createdDate": "2025-09-25T14:09:45+02:00",
                "field": "NEW",
                "value": {
                    "from": null,
                    "to": null
                },
                "user": {
                    "id": 503,
                    "name": "Maria",
                    "lastName": "Ivanova",
                    "secondName": "",
                    "login": "maria@mysite.com"
                }
            },
            {
                "id": 16361,
                "createdDate": "2025-09-25T14:09:45+02:00",
                "field": "COMMENT",
                "value": {
                    "from": null,
                    "to": "3409"
                },
                "user": {
                    "id": 503,
                    "name": "Maria",
                    "lastName": "Ivanova",
                    "secondName": "",
                    "login": "maria@mysite.com"
                }
            },
            {
                "id": 16363,
                "createdDate": "2025-09-25T14:09:45+02:00",
                "field": "CHECKLIST_ITEM_CREATE",
                "value": {
                    "from": "",
                    "to": "What to do"
                },
                "user": {
                    "id": 503,
                    "name": "Maria",
                    "lastName": "Ivanova",
                    "secondName": "",
                    "login": "maria@mysite.com"
                }
            },
            {
                "id": 16365,
                "createdDate": "2025-09-25T14:09:45+02:00",
                "field": "CHECKLIST_ITEM_CREATE",
                "value": {
                    "from": "",
                    "to": "Contact the client"
                },
                "user": {
                    "id": 503,
                    "name": "Maria",
                    "lastName": "Ivanova",
                    "secondName": "",
                    "login": "maria@mysite.com"
                }
            },
            {
                "id": 16367,
                "createdDate": "2025-09-25T14:09:45+02:00",
                "field": "CHECKLIST_ITEM_CREATE",
                "value": {
                    "from": "",
                    "to": "Prepare the contract"
                },
                "user": {
                    "id": 503,
                    "name": "Maria",
                    "lastName": "Ivanova",
                    "secondName": "",
                    "login": "maria@mysite.com"
                }
            },
            {
                "id": 16369,
                "createdDate": "2025-09-25T14:09:45+02:00",
                "field": "CHECKLIST_ITEM_CREATE",
                "value": {
                    "from": "",
                    "to": "Sign the contract"
                },
                "user": {
                    "id": 503,
                    "name": "Maria",
                    "lastName": "Ivanova",
                    "secondName": "",
                    "login": "maria@mysite.com"
                }
            },
            {
                "id": 16371,
                "createdDate": "2025-09-25T14:09:57+02:00",
                "field": "AUDITORS",
                "value": {
                    "from": "",
                    "to": "547"
                },
                "user": {
                    "id": 503,
                    "name": "Maria",
                    "lastName": "Ivanova",
                    "secondName": "",
                    "login": "maria@mysite.com"
                }
            },
            {
                "id": 16375,
                "createdDate": "2025-09-25T14:09:57+02:00",
                "field": "COMMENT",
                "value": {
                    "from": null,
                    "to": "3411"
                },
                "user": {
                    "id": 503,
                    "name": "Maria",
                    "lastName": "Ivanova",
                    "secondName": "",
                    "login": "maria@mysite.com"
                }
            },
            {
                "id": 16373,
                "createdDate": "2025-09-25T14:09:58+02:00",
                "field": "COMMENT",
                "value": {
                    "from": null,
                    "to": "3413"
                },
                "user": {
                    "id": 503,
                    "name": "Maria",
                    "lastName": "Ivanova",
                    "secondName": "",
                    "login": "maria@mysite.com"
                }
            }
        ],
    },
    "time": {
        "start": 1758798620,
        "finish": 1758798620.969019,
        "duration": 0.9690189361572266,
        "processing": 0,
        "date_start": "2025-09-25T14:10:20+02:00",
        "date_finish": "2025-09-25T14:10:20+02:00",
        "operating_reset_at": 1758799220,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | The root element of the response. Contains an array `list`, which includes objects with [event descriptions](#lists) for the task.

Returns an empty array `"list":[]` if the task does not exist ||
|| **time**
[`time`](../data-types.md#time) | Information about the time taken for the request ||
|#

#### Objects lists {#lists}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the history event ||
|| **createdDate**
[`string`](../data-types.md) | Date and time of the event creation in ISO 8601 format ||
|| **field**
[`string`](../data-types.md) | Type of history event. Possible values for `field`:

- `TITLE` — change of task title
- `DESCRIPTION` — change of task description
- `REAL_STATUS` — change of actual status
- `STATUS` — change of task status
- `PRIORITY` — change of priority
- `MARK` — change of task rating
- `COMMENT` — addition of a comment
- `DELETE` — deletion of a task
- `NEW` — creation of a new task
- `RENEW` — restoration of a task
- `MOVE_TO_BACKLOG` — moving a task to the backlog
- `MOVE_TO_SPRINT` — moving a task to a sprint
- `PARENT_ID` — change of parent task
- `GROUP_ID` — change of workgroup/project
- `STAGE_ID` — change of stage
- `CREATED_BY` — change of task author
- `RESPONSIBLE_ID` — change of responsible person
- `ACCOMPLICES` — change of participants
- `AUDITORS` — change of observers
- `DEADLINE` — change of deadline
- `START_DATE_PLAN` — change of planned start date
- `END_DATE_PLAN` — change of planned end date
- `DURATION_PLAN` — change of planned duration
- `DURATION_PLAN_SECONDS` — change of planned duration in seconds
- `DURATION_FACT` — change of actual duration
- `TIME_ESTIMATE` — change of time estimate
- `TIME_SPENT_IN_LOGS` — change of actual time spent in logs
- `TAGS` — change of task tags
- `DEPENDS_ON` — change of task dependencies
- `FILES` — change of file list
- `UF_TASK_WEBDAV_FILES` — change of user field with files
- `CHECKLIST_ITEM_CREATE` — creation of a checklist item
- `CHECKLIST_ITEM_RENAME` — renaming of a checklist item
- `CHECKLIST_ITEM_REMOVE` — removal of a checklist item
- `CHECKLIST_ITEM_CHECK` — marking a checklist item as completed
- `CHECKLIST_ITEM_UNCHECK` — unchecking a checklist item
- `ADD_IN_REPORT` — change of the "add to report" flag
- `TASK_CONTROL` — change of result control
- `ALLOW_TIME_TRACKING` — enabling or disabling time tracking
- `ALLOW_CHANGE_DEADLINE` — allowing or prohibiting deadline changes
- `FLOW_ID` — change of flow ||
|| **value**
[`object`](../data-types.md) | The object describes what change occurred:
- `from` — value before the change
- `to` — value after the change

The type of value depends on the event: for a new comment — `ID` of the comment, for a checklist item change — text of the item, for adding an observer — user identifier, and so on ||
|| **user**
[`object`](../data-types.md) | An object with [user description](#user) who performed the action ||
|#

#### User Object {#user}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | User identifier ||
|| **name**
[`string`](../data-types.md) | First name ||
|| **lastName**
[`string`](../data-types.md) | Last name ||
|| **secondName**
[`string`](../data-types.md) | Middle name ||
|| **login**
[`string`](../data-types.md) | Login ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "Access denied. (internal error)"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `100` | CTaskItem All parameters in the constructor must have real class type (internal error) | Required parameter `taskId` is missing ||
|| `0` | wrong task id (internal error) | The `taskId` parameter has an invalid type ||
|| `0` | Access denied. (internal error) | The user does not have access to the task ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./tasks-task-get.md)
- [{#T}](./tasks-task-list.md)