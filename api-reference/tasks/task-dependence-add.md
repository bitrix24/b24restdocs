# Create a Dependency Between Tasks task.dependence.add

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user

This method creates a dependency of one task on another.

## Method Parameters

{% include [Note on Required Parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **taskIdFrom***
[`integer`](../data-types.md) | The identifier of the task from which the dependency is created ||
|| **taskIdTo***
[`integer`](../data-types.md) | The identifier of the task for which the dependency is created ||
|| **linkType***
[`integer`](../data-types.md) | Type of dependency:
- `0` — start-start link 
- `1` — start-finish link 
- `2` — finish-start link 
- `3` — finish-finish link 
||
|#

The task identifier can be obtained when [creating a new task](./tasks-task-add.md) or by using the [method to get the list of tasks](./tasks-task-list.md).

## Code Examples

{% include [Note on Examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskIdFrom":100,"taskIdTo":101,"linkType":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.dependence.add
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskIdFrom":100,"taskIdTo":101,"linkType":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.dependence.add
    ```

- JS

    ```js
    BX24.callMethod(
        'task.dependence.add', {
            "taskIdFrom":100,
            "taskIdTo":101,
            "linkType":0
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

    $result = CRest::call('task.dependence.add', [
        'taskIdFrom' => 100,
        'taskIdTo' => 101,
        'linkType' => 0,
    ]);

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

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

HTTP Status: **400**

```json
{
    "error":"ILLEGAL_NEW_LINK",
    "error_description":"The link between tasks already exists"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ILLEGAL_NEW_LINK` | The link between tasks already exists ||
|| `ACTION_NOT_ALLOWED` | It is not possible to add a link between tasks ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./task-dependence-delete.md)