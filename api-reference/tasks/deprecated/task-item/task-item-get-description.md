# Get Task Description task.item.getdescription

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns the description of the task.

{% note warning %}

The method is deprecated and not supported. It is recommended to use the methods [tasks.task.*](../../index.md).

{% endnote %}

## Method Parameters

#|
|| **Name** | **Description** ||
|| **TASKID** | Task identifier ||
|| **FORMAT** | Acceptable values:
- `1` (corresponds to PHP constant `CTaskItem::DESCR_FORMAT_RAW`) — the description will be returned in the format it is stored in the database (HTML or BB-code), no sanitization will be performed
- `2` (corresponds to PHP constant `CTaskItem::DESCR_FORMAT_HTML`) — the description will be returned in HTML format, sanitized (if enabled in task module settings)
- `3` (corresponds to PHP constant `CTaskItem::DESCR_FORMAT_PLAIN_TEXT`) — the description will be returned as "plain" text (without HTML tags) ||
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
    -d '{"TASKID":13,"USERID":1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.item.getdescription
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":13,"USERID":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.item.getdescription
    ```

- JS

    ```js
    BX24.callMethod(
        'task.item.getdescription',
        [13, 1],
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
        'task.item.getdescription',
        [
            'TASKID' => 13,
            'USERID' => 1
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}