# Delete the dependency between tasks task.dependence.delete

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user

This method removes the dependency of one task on another.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **taskIdFrom***
[`integer`](../data-types.md) | The identifier of the task from which the dependency is being removed. The task identifier is returned by the [method for creating a new task](./tasks-task-add.md) ||
|| **taskIdTo***
[`integer`](../data-types.md) | The identifier of the task for which the dependency is being removed ||
|#

The task identifier can be obtained when [creating a new task](./tasks-task-add.md) or by using the [method for retrieving the list of tasks](./tasks-task-list.md).

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskIdFrom":100,"taskIdTo":101}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.dependence.delete
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskIdFrom":100,"taskIdTo":101,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.dependence.delete
    ```

- JS

    ```js
    BX24.callMethod(
        'task.dependence.delete', {
            "taskIdFrom":100,
            "taskIdTo":101,
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
        'task.dependence.delete',
        [
            'taskIdFrom' => 100,
            'taskIdTo' => 101,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

Example of a successfully executed request

```json
{
    "result":{
        []
    },
    "time":{
        "start":1712137817.343984,
        "finish":1712137817.605804,
        "duration":0.26182007789611816,
        "processing":0.018325090408325195,
        "date_start":"2024-04-03T12:50:17+03:00",
        "date_finish":"2024-04-03T12:50:17+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | In case of success, contains an empty array ||
|| **time**
[`time`](../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"ILLEGAL_NEW_LINK",
    "error_description":"The dependency between tasks does not exist"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ILLEGAL_NEW_LINK` | The dependency between tasks does not exist ||
|| `ACTION_NOT_ALLOWED` | It is not possible to delete the dependency between tasks ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./task-dependence-add.md)