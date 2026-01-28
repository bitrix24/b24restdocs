# Move Checklist Item task.checklistitem.moveafteritem

> Scope: [`task`](../../scopes/permissions.md)
>
> Permissions to execute the method:
> - any user with edit access to the task
> - Creator, Performer, and Participants of the task

The method `task.checklistitem.moveafteritem` moves the checklist item `itemId` to a position after the element `afterItemId`.

Both elements must belong to the same task `taskId`. The elements can be in different sublists, but after the move, `itemId` will have the same `PARENT_ID` as `afterItemId`.

You can check permissions to modify the item using the method [task.checklistitem.isactionallowed](./task-checklist-item-is-action-allowed.md).

## Method Parameters

{% include [Required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TASKID***
[`integer`](../../data-types.md) | Identifier of the task.

The task identifier can be obtained when [creating a new task](../tasks-task-add.md) or by using the method [get task list](../tasks-task-list.md) ||
|| **ITEMID***
[`integer`](../../data-types.md) | Identifier of the checklist item being moved.

The checklist item identifier can be obtained when [creating an item](./task-checklist-item-add.md) or by using the method [get checklist item list](./task-checklist-item-get-list.md) ||
|| **AFTERITEMID***
[`integer`](../../data-types.md) | Identifier of the checklist item after which the moving item should be placed.

The item must belong to the same task as `ITEMID`.

The checklist item identifier can be obtained when [creating an item](./task-checklist-item-add.md) or by using the method [get checklist item list](./task-checklist-item-get-list.md) ||
|#

## Code Examples

{% include [Required parameters in examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":13,"ITEMID":475,"AFTERITEMID":447}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.checklistitem.moveafteritem
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":13,"ITEMID":475,"AFTERITEMID":447,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.checklistitem.moveafteritem
    ```

- JS

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'task.checklistitem.moveafteritem',
            {
                TASKID: 13,
                ITEMID: 475,
                AFTERITEMID: 447
            }
        );
        
        const result = response.getData().result;
        console.log('Moved checklist item:', result);
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
                'task.checklistitem.moveafteritem',
                [
                    'TASKID' => 13,
                    'ITEMID' => 475,
                    'AFTERITEMID' => 447
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error moving checklist item: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.checklistitem.moveafteritem',
        {
            TASKID: 13,
            ITEMID: 475,
            AFTERITEMID: 447
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
        'task.checklistitem.moveafteritem',
        [
            'TASKID' => 13,
            'ITEMID' => 475,
            'AFTERITEMID' => 447
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
    "result": null,
    "time": {
        "start": 1764597401,
        "finish": 1764597401.936492,
        "duration": 0.9364919662475586,
        "processing": 0,
        "date_start": "2025-12-01T16:56:41+01:00",
        "date_finish": "2025-12-01T16:56:41+01:00",
        "operating_reset_at": 1764598001,
        "operating": 0.29050707817077637
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
`null` | Returns `null` if the item was successfully moved ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_CORE",
    "error_description": "TASKS_ERROR_EXCEPTION_#8; Moving item: action not allowed; 8/TE/ACTION_FAILED_TO_BE_PROCESSED<br>"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value**  ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #1 (itemId) expected by method ctaskchecklistitem::moveafteritem(), but not given.; 256/TE/WRONG_ARGUMENTS<br> | Required parameter `TASKID`, `ITEMID`, or `AFTERITEMID` is missing ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #0 (taskId) for method ctaskchecklistitem::moveafteritem() expected to be of type "integer", but given something else.; 256/TE/WRONG_ARGUMENTS<br> | Incorrect type provided for `TASKID`, `ITEMID`, or `AFTERITEMID` ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#8; Moving item: action not allowed; 8/TE/ACTION_FAILED_TO_BE_PROCESSED<br> | User does not have access to the task or lacks permissions to perform the action ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./task-checklist-item-add.md)
- [{#T}](./task-checklist-item-update.md)
- [{#T}](./task-checklist-item-get.md)
- [{#T}](./task-checklist-item-get-list.md)
- [{#T}](./task-checklist-item-delete.md)
- [{#T}](./task-checklist-item-complete.md)
- [{#T}](./task-checklist-item-renew.md)
- [{#T}](./task-checklist-item-is-action-allowed.md)