# Check if the action task.item.isactionallowed is permitted

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns `true` if the action is allowed. Otherwise, it will return `false`.

{% note warning %}

The method is deprecated and not supported. It is recommended to use the methods [tasks.task.*](../../index.md).

{% endnote %}

## Method Parameters

#|
|| **Name** | **Description** ||
|| **TASKID** | Task identifier ||
|| **ACTIONID** | Identifier of the action being checked (see the constants of the method [task.item.getallowedactions](./task-item-get-allowed-actions.md)) ||
|#

It is mandatory to follow the order of parameters in the request. If this order is violated, the request will be executed with errors.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":13,"ACTION":6}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.item.isactionallowed
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":13,"ACTION":6,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.item.isactionallowed
    ```

- JS

    ```js
    BX24.callMethod(
        'task.item.isactionallowed',
        [13, 6],
        function(result)
        {
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'task.item.isactionallowed',
        [
            'TASKID' => 13,
            'ACTION' => 6
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}