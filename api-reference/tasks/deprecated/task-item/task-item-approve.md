# Translate a pending control task to "completed" status task.item.approve

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method changes the status of a pending control task to "completed".

{% note warning %}

This method is deprecated and not supported. It is recommended to use the methods [tasks.task.*](../../index.md).

{% endnote %}

## Method Parameters

#|
|| **Name** | **Description** ||
|| **TASKID** | Task identifier ||
|#

## Code Examples

{% include [Note about examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":13}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.item.approve
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":13,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.item.approve
    ```

- JS

    ```js
    BX24.callMethod(
        'task.item.approve',
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
        'task.item.approve',
        [
            'TASKID' => 13
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}