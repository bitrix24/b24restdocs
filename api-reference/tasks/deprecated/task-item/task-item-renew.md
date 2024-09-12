# Change Task Status to "Not Completed" task.item.renew

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method changes the task status to "Not Completed". It is applicable for tasks with the status [Completed](./task-item-complete.md).

{% note warning %}

This method is deprecated and not supported. It is recommended to use the methods [tasks.task.*](../../index.md).

{% endnote %}

## Method Parameters

#|
|| **Name** | **Description** ||
|| **TASKID** | Task identifier ||
|#

## Code Examples

{% include [Example Note](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskId":13}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.item.renew
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskId":13,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.item.renew
    ```

- JS

    ```js
    BX24.callMethod(
        'task.item.renew',
        [13],
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
        'task.item.renew',
        ['taskId' => 13]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}