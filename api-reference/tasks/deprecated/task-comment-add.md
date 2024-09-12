# Add Comment to Task task.comment.add

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method adds comments to a task.

{% note warning %}

Instead of this method, you should use the methods [`task.commentitem.*`](../comment-item/index.md).

{% endnote %}

## Method Parameters

#|
|| **Name** | **Description** ||
|| **TASKID** | Task identifier ||
|| **COMMENTTEXT** | Comment ||
|#

It is essential to maintain the order of parameters in the request. If this order is violated, the request will be executed with errors.

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":1,"FIELDS":{"POST_MESSAGE":"comment text"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.comment.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":1,"FIELDS":{"POST_MESSAGE":"comment text"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.comment.add
    ```

- JS

    ```js
    BX24.callMethod(
        'task.comment.add',
        [1, 'comment text'],
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
        'task.comment.add',
        [
            'TASKID' => 1,
            'FIELDS' => [
                'POST_MESSAGE' => 'comment text'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}