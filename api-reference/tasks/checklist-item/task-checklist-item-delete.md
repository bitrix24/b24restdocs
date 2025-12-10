# Delete checklist item task.checklistitem.delete

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method:
> - any user with editing access to the task
> - creator, assignee, and participants of the task

The method `task.checklistitem.delete` removes a checklist item from a task.

You can check permissions for deleting an item using the method [task.checklistitem.isactionallowed](./task-checklist-item-is-action-allowed.md).

## Method Parameters

{% include [Required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TASKID***
[`integer`](../../data-types.md) | Task identifier.

The task identifier can be obtained when [creating a new task](../tasks-task-add.md) or using the method [getting the list of tasks](../tasks-task-list.md)  ||
|| **ITEMID***
[`integer`](../../data-types.md) | Checklist item identifier.

The item identifier can be obtained when [adding a new item](./task-checklist-item-add.md) or using the method [getting the list of checklist items](./task-checklist-item-get-list.md) ||
|#

## Code Examples

{% include [Required parameters in examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":13,"ITEMID":475}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.checklistitem.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":13,"ITEMID":475,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.checklistitem.delete
    ```

- JS

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'task.checklistitem.delete',
            {
                TASKID: 13,
                ITEMID: 475
            }
        );
        
        const result = response.getData().result;
        console.log('Deleted checklist item:', result);
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
                'task.checklistitem.delete',
                [
                    'TASKID' => 13,
                    'ITEMID' => 475
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting checklist item: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.checklistitem.delete',
        {
            TASKID: 13,
            ITEMID: 475
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
        'task.checklistitem.delete',
        [
            'TASKID' => 13,
            'ITEMID' => 475
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
        "start": 1764594534,
        "finish": 1764594535.201817,
        "duration": 1.2018170356750488,
        "processing": 0,
        "date_start": "2025-12-01T16:08:54+01:00",
        "date_finish": "2025-12-01T16:08:55+01:00",
        "operating_reset_at": 1764595135,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if the checklist item was successfully deleted ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_CORE",
    "error_description": "TASKS_ERROR_EXCEPTION_#8; Deleting item: action not allowed; 8/TE/ACTION_FAILED_TO_BE_PROCESSED<br>"
}
```

{% include notitle [Error response](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Value** | **How to resolve**  ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #1 (itemId) expected by method ctaskchecklistitem::delete(), but not given.; 256/TE/WRONG_ARGUMENTS<br> | Required parameter `TASKID` or `ITEMID` not specified ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #0 (taskId) for method ctaskchecklistitem::delete() expected to be of type "integer", but given something else.; 256/TE/WRONG_ARGUMENTS<br> | Incorrect value type for `TASKID` or `ITEMID` specified ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#8; Deleting item: action not allowed; 8/TE/ACTION_FAILED_TO_BE_PROCESSED<br> | User does not have access to the task or lacks permissions to perform the action ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./task-checklist-item-add.md)
- [{#T}](./task-checklist-item-update.md)
- [{#T}](./task-checklist-item-get.md)
- [{#T}](./task-checklist-item-get-list.md)
- [{#T}](./task-checklist-item-move-after-item.md)
- [{#T}](./task-checklist-item-complete.md)
- [{#T}](./task-checklist-item-renew.md)
- [{#T}](./task-checklist-item-is-action-allowed.md)