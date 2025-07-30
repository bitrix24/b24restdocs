# Check Action Permission for task.elapseditem.isactionallowed

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method checks whether an action is permitted on a record: creation, modification, and deletion.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **TASKID***  
[`integer`](../../data-types.md) | Task identifier.

The task identifier can be obtained when [creating a new task](../tasks-task-add.md) or by using the [method to get the list of tasks](../tasks-task-list.md) ||
|| **ITEMID***  
[`integer`](../../data-types.md) | Identifier of the elapsed time record.

It can be obtained when [creating a new record](./task-elapsed-item-add.md) or by using the [method to get the list of elapsed time records](./task-elapsed-item-get-list.md) ||
|| **ACTIONID***  
[`integer`](../../data-types.md) | Identifier of the action:
- **1** — add a new record (`ACTION_ELAPSED_TIME_ADD`)
- **2** — modify a record (`ACTION_ELAPSED_TIME_MODIFY`)
- **3** — delete a record (`ACTION_ELAPSED_TIME_REMOVE`) ||
|#

{% note warning %}

It is mandatory to follow the order of parameters in the request as specified in the table. Otherwise, the request will execute with errors.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID" : 691,"ITEMID": 5,"ACTIONID": 1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.elapseditem.isActionAllowed
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID" : 691,"ITEMID": 5,"ACTIONID": 1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.elapseditem.isActionAllowed
    ```

- JS

    ```js
    BX24.callMethod(
        'task.elapseditem.isActionAllowed',
        {
            "TASKID" : 691,
            "ITEMID": 5,
            "ACTIONID": 1,
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'task.elapseditem.isActionAllowed',
        [
            'TASKID' => 691,
            'ITEMID' => 5,
            'ACTIONID' => 1,
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
        "start": 1712137817.343984,
        "finish": 1712137817.605804,
        "duration": 0.26182007789611816,
        "processing": 0.018325090408325195,
        "date_start": "2024-04-03T12:50:17+02:00",
        "date_finish": "2024-04-03T12:50:17+02:00"
    }
}
```

### Returned Data

#|
|| **Name**  
`type` | **Description** ||
|| **result**  
[`boolean`](../../data-types.md) | Result of the permission check for the action:
- `true` — permitted
- `false` — not permitted
 ||
|| **time**  
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./task-elapsed-item-add.md)
- [{#T}](./task-elapsed-item-update.md)
- [{#T}](./task-elapsed-item-get.md)
- [{#T}](./task-elapsed-item-get-list.md)
- [{#T}](./task-elapsed-item-delete.md)
- [{#T}](./task-elapsed-item-get-manifest.md)