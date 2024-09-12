# Delegate Task task.item.delegate

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method delegates a task to a new user.

{% note warning %}

This method is deprecated and not supported. It is recommended to use the methods [tasks.task.*](../../index.md).

{% endnote %}

## Method Parameters

#|
|| **Name** | **Description** ||
|| **TASKID** | Task identifier ||
|| **USERID** | Identifier of the new assignee (responsible) ||
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
    -d '{"TASKID":13,"USERID":3}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.item.delegate
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":13,"USERID":3,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.item.delegate
    ```

- JS

    ```js
    BX24.callMethod(
        'task.item.delegate',
        [13, 3],
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
        'task.item.delegate',
        [
            'TASKID' => 13,
            'USERID' => 3
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}