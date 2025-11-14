# Add checklist item task.checklistitem.add

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method:
> - any user with access to edit the task
> - creator, assignee, and participants of the task

The method `task.checklistitem.add` adds a new checklist item to a task.

You can check permissions for adding an item using the method [task.checklistitem.isactionallowed](./task-checklist-item-is-action-allowed.md).

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TASKID***
[`integer`](../../data-types.md) | Task identifier.

The task identifier can be obtained when [creating a new task](../tasks-task-add.md) or using the [get task list method](../tasks-task-list.md) ||
|| **FIELDS***
[`object`](../../data-types.md) | Object with [checklist item fields](#fields) ||
|#

### FIELDS Parameter {#fields}

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TITLE***
[`string`](../../data-types.md) | Text of the checklist item.

If `PARENT_ID` is passed with a value of `0`, then `TITLE` is the name of the checklist ||
|| **SORT_INDEX**
[`integer`](../../data-types.md) | Sort index. The lower the value, the higher the item in the list or sublist ||
|| **IS_COMPLETE**
[`boolean`](../../data-types.md) | Status of the item. Possible values:
- `Y` — completed
- `N` — not completed

Default is `N` ||
|| **IS_IMPORTANT**
[`boolean`](../../data-types.md) | Mark indicating that the item is important. Possible values:
- `Y` — important
- `N` — regular ||
|| **MEMBERS**
[`object`](../../data-types.md) | Object describing the participants of the checklist item. Key — user identifier, value — object with participant type parameter `TYPE`. Possible participant type values:
- `'TYPE': 'A'` — participant
- `'TYPE': 'U'` — observer

The system will add checklist item participants to the task in the same roles
 ||
|| **PARENT_ID**
[`integer`](../../data-types.md) | Identifier of the parent item. Use for nested checklists.

- If `PARENT_ID` is passed with a value of `0`, the system will create a new checklist in the task
- If there is no checklist item in the task with the specified `PARENT_ID`, the system will create a new checklist
- If `PARENT_ID` is not specified in `FIELDS`, the system will add a new item to the existing top-level checklist. If there is no checklist in the task, it will create a new one

||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":13,"FIELDS":{"TITLE":"Prepare report","PARENT_ID":457,"SORT_INDEX":200,"IS_COMPLETE":"N","IS_IMPORTANT":"Y","MEMBERS":{"547":{"TYPE":"A"}}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.checklistitem.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":13,"FIELDS":{"TITLE":"Prepare report","PARENT_ID":457,"SORT_INDEX":200,"IS_COMPLETE":"N","IS_IMPORTANT":"Y","MEMBERS":{"547":{"TYPE":"A"}}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.checklistitem.add
    ```

- JS

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'task.checklistitem.add',
            {
                TASKID: 13,
                FIELDS: {
                    TITLE: 'Prepare report',
                    PARENT_ID: 457,
                    SORT_INDEX: 200,
                    IS_COMPLETE: 'N',
                    IS_IMPORTANT: 'Y',
                    MEMBERS: {
                        547: {
                            TYPE: 'A'
                        }
                    }
                }
            }
        );
        
        const result = response.getData().result;
        console.log('Created checklist item with ID:', result);
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
                'task.checklistitem.add',
                [
                    'TASKID' => 13,
                    'FIELDS' => [
                        'TITLE' => 'Prepare report',
                        'PARENT_ID' => 457,
                        'SORT_INDEX' => 200,
                        'IS_COMPLETE' => 'N',
                        'IS_IMPORTANT' => 'Y',
                        'MEMBERS' => [
                            547 => [
                                'TYPE' => 'A'
                            ]
                        ]
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding checklist item: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.checklistitem.add',
        {
            'TASKID': 13,
            'FIELDS': {
                'TITLE': 'Prepare report',
                'PARENT_ID': 457,
                'SORT_INDEX': 200,
                'IS_COMPLETE': 'N',
                'IS_IMPORTANT': 'Y',
                'MEMBERS': {
                    547: {
                        'TYPE': 'A'
                    }
                }
            }
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
        'task.checklistitem.add',
        [
            'TASKID' => 13,
            'FIELDS' => [
                'TITLE' => 'Prepare report',
                'PARENT_ID' => 457,
                'SORT_INDEX' => 200,
                'IS_COMPLETE' => 'N',
                'IS_IMPORTANT' => 'Y',
                'MEMBERS' => [
                    547 => [
                        'TYPE' => 'A'
                    ]
                ]
            ]
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
    "result": 475,
    "time": {
        "start": 1762431907,
        "finish": 1762431908.259832,
        "duration": 1.2598319053649902,
        "processing": 0,
        "date_start": "2025-11-06T15:25:07+02:00",
        "date_finish": "2025-11-06T15:25:08+02:00",
        "operating_reset_at": 1762432508,
        "operating": 0.24803590774536133
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../data-types.md) | Identifier of the new checklist item ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"ERROR_CORE",
    "error_description":"TASKS_ERROR_EXCEPTION_#8; Adding item: action not allowed; 8/TE/ACTION_FAILED_TO_BE_PROCESSED<br>"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value**  ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#8; Adding item: action not allowed; 8/TE/ACTION_FAILED_TO_BE_PROCESSED<br> | No access to the task or insufficient permissions to work with checklists in the task ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #0 (taskId) for method ctaskchecklistitem::add() expected to be of type "integer", but given something else.; 256/TE/WRONG_ARGUMENTS | Required parameter `TASKID` not provided or incorrect type specified for `TASKID` ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #1 (arFields) expected by method ctaskchecklistitem::add(), but not given.; 256/TE/WRONG_ARGUMENTS<br> | Required parameter `FIELDS` not provided or empty ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#8; Item name not specified; 8/TE/ACTION_FAILED_TO_BE_PROCESSED<br> | Required field `TITLE` not provided in the `FIELDS` parameter ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./task-checklist-item-update.md)
- [{#T}](./task-checklist-item-get.md)
- [{#T}](./task-checklist-item-get-list.md)
- [{#T}](./task-checklist-item-delete.md)
- [{#T}](./task-checklist-item-move-after-item.md)
- [{#T}](./task-checklist-item-complete.md)
- [{#T}](./task-checklist-item-renew.md)
- [{#T}](./task-checklist-item-is-action-allowed.md)