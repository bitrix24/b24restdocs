# Check Action Permission for task.checklistitem.isactionallowed

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.checklistitem.isactionallowed` checks whether an action is permitted for a checklist item in a task.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TASKID***
[`integer`](../../data-types.md) | Task identifier.

The task identifier can be obtained when [creating a new task](../tasks-task-add.md) or by using the [method to get the task list](../tasks-task-list.md) ||
|| **ITEMID***
[`integer`](../../data-types.md) | Checklist item identifier.

The item identifier can be obtained when [adding a new item](./task-checklist-item-add.md) or by using the [method to get the checklist item list](./task-checklist-item-get-list.md) ||
|| **ACTIONID***
[`integer`](../../data-types.md) | Identifier of the action being checked:
- `1` — add item `ACTION_ADD`
- `2` — modify item `ACTION_MODIFY`
- `3` — delete item `ACTION_REMOVE`
- `4` — mark as complete `ACTION_TOGGLE`
- `5` — move item `ACTION_REORDER` ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":8017,"ITEMID":475,"ACTIONID":2}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.checklistitem.isactionallowed
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":8017,"ITEMID":475,"ACTIONID":2,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.checklistitem.isactionallowed
    ```

- JS

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'task.checklistitem.isactionallowed',
            {
                TASKID: 8017,
                ITEMID: 475,
                ACTIONID: 2
            }
        );
        
        const result = response.getData().result;
        console.log('Action allowed:', result);
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
                'task.checklistitem.isactionallowed',
                [
                    'TASKID' => 8017,
                    'ITEMID' => 475,
                    'ACTIONID' => 2
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error checking action: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.checklistitem.isactionallowed',
        {
            'TASKID': 8017,
            'ITEMID': 475,
            'ACTIONID': 2
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
        'task.checklistitem.isactionallowed',
        [
            'TASKID' => 8017,
            'ITEMID' => 475,
            'ACTIONID' => 2
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
        "start": 1769070724,
        "finish": 1769070724.446313,
        "duration": 0.44631290435791016,
        "processing": 0,
        "date_start": "2026-01-22T11:32:04+01:00",
        "date_finish": "2026-01-22T11:32:04+01:00",
        "operating_reset_at": 1769071324,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Result of the check:
- `true` — action is allowed
- `false` — action is not allowed or non-existent identifiers were provided ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":"ERROR_CORE",
    "error_description":"TASKS_ERROR_EXCEPTION_#256; Param #2 (actionId) expected by method ctaskchecklistitem::isactionallowed(), but not given.; 256/TE/WRONG_ARGUMENTS"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #2 (actionId) expected by method ctaskchecklistitem::isactionallowed(), but not given.; 256/TE/WRONG_ARGUMENTS | Required parameter not specified: `TASKID`, `ITEMID` or `ACTIONID` ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #0 (taskId) for method ctaskchecklistitem::isactionallowed() expected to be of type "integer", but given something else.; 256/TE/WRONG_ARGUMENTS | Incorrect value type provided for parameters `TASKID`, `ITEMID` or `ACTIONID` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./task-checklist-item-add.md)
- [{#T}](./task-checklist-item-update.md)
- [{#T}](./task-checklist-item-get.md)
- [{#T}](./task-checklist-item-get-list.md)
- [{#T}](./task-checklist-item-delete.md)
- [{#T}](./task-checklist-item-move-after-item.md)
- [{#T}](./task-checklist-item-complete.md)
- [{#T}](./task-checklist-item-renew.md)