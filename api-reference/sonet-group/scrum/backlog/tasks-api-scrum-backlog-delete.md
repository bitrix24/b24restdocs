# Delete Backlog tasks.api.scrum.backlog.delete

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.api.scrum.backlog.delete` removes the backlog.

In normal circumstances, there is no need to delete the backlog. When the backlog is deleted, *Bitrix24* will automatically recreate it when the planning page in Scrum tasks is opened.

This method is used if the backlog was mistakenly added to a group/project.

## Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Parameter** / **Type** | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Backlog identifier. It can be obtained when creating [tasks.api.scrum.backlog.add](./tasks-api-scrum-backlog-add.md) or [tasks.api.scrum.backlog.get](./tasks-api-scrum-backlog-get.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id": 1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.api.scrum.backlog.delete
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id": 1, "auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/tasks.api.scrum.backlog.delete
    ```

- JS

    ```js
    BX24.callMethod(
        'tasks.api.scrum.backlog.delete',{
            "id": 1
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

    $result = CRest::call('tasks.api.scrum.backlog.delete', [
        'id' => 1,
    ]);

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

In case of successful execution, the server will return the following response:

```json
{
    "result": [],
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

## Error Handling

HTTP status: **400**

```json
{
    "error":0,
    "error_description":"Access denied"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| `0` | Backlog not found | This error occurs when trying to delete a non-existent backlog ||
|| `0` | Access denied | There are insufficient access permissions ||
|| `0` | Unknown error | Another error occurred ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-api-scrum-backlog-add.md)
- [{#T}](./tasks-api-scrum-backlog-update.md)
- [{#T}](./tasks-api-scrum-backlog-get.md)
- [{#T}](./tasks-api-scrum-backlog-get-fields.md)