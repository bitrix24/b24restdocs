# Delete Message im.message.delete

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: the author of the message or an administrator in a group chat

The method `im.message.delete` removes a message.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **MESSAGE_ID***
[`integer`](../../data-types.md) | Identifier of the message.

The identifier can be obtained using the method [im.dialog.messages.get](./im-dialog-messages-get.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"MESSAGE_ID":34247}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.message.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"MESSAGE_ID":34247,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/im.message.delete
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'im.message.delete',
            {
                MESSAGE_ID: 34247
            }
        );
        
        const result = response.getData().result;
        console.log(result);
        processResult(result);
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'im.message.delete',
                [
                    'MESSAGE_ID' => 34247
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting message: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.message.delete',
        {
            MESSAGE_ID: 34247
        },
        function(result)
        {
            if (result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'im.message.delete',
        [
            'MESSAGE_ID' => 34247
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
        "start": 1772629211,
        "finish": 1772629211.717344,
        "duration": 0.7173440456390381,
        "processing": 0,
        "date_start": "2026-03-04T16:00:11+01:00",
        "date_finish": "2026-03-04T16:00:11+01:00",
        "operating_reset_at": 1772629811,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | `true` if the message was deleted ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "MESSAGE_ID_ERROR",
    "error_description": "Message ID can't be empty"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `MESSAGE_ID_ERROR` | Message ID can't be empty | `MESSAGE_ID` is missing or invalid ||
|| `CANT_EDIT_MESSAGE` | Time has expired for modification or you don't have access | No permission to delete the message or the editing time has expired ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-message-add.md)
- [{#T}](./im-message-update.md)
- [{#T}](./im-message-like.md)