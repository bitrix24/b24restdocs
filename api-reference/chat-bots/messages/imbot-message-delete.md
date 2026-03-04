# Delete Message imbot.message.delete

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: an authorized user of the application that registered the chat bot. The method works only with bots of this application.

The method `imbot.message.delete` removes a message from the chat bot.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **BOT_ID**
[`integer`](../../data-types.md) | The identifier of the chat bot. You can obtain the bot ID using the method [imbot.bot.list](../imbot-bot-list.md).

If the parameter is not provided, the method searches for the first bot registered by the current application. ||
|| **MESSAGE_ID*** 
[`integer`](../../data-types.md) | The identifier of the message to be deleted. The value must be greater than `0`.

For messages sent by the bot via REST, the identifier is returned by the method [imbot.message.add](./imbot-message-add.md). ||
|| **COMPLETE**
[`string`](../../data-types.md) | Deletion mode.

Allowed values:
- `Y` — delete completely, without marking as deleted
- `N` — standard deletion, default value. ||
|| **CLIENT_ID**
[`string`](../../data-types.md) | Technical parameter for scenarios without `clientId` in authorization.

If provided, it is used as `custom{CLIENT_ID}` to identify the application. ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"BOT_ID":39,"MESSAGE_ID":19880117,"COMPLETE":"N"}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.message.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"BOT_ID":39,"MESSAGE_ID":19880117,"COMPLETE":"N","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imbot.message.delete
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.message.delete', {
        BOT_ID: 39,
        MESSAGE_ID: 19880117,
        COMPLETE: 'N',
      });

      const { result } = response.getData();
      console.log('Deleted:', result);
    } catch (error) {
      console.error('Error deleting message:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'imbot.message.delete',
                [
                    'BOT_ID' => 39,
                    'MESSAGE_ID' => 19880117,
                    'COMPLETE' => 'N',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Deleted: ' . ($result->data() ? 'true' : 'false');
        }
    } catch (Throwable $exception) {
        error_log($exception->getMessage());
        echo 'Error deleting message: ' . $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.message.delete',
        {
            BOT_ID: 39,
            MESSAGE_ID: 19880117,
            COMPLETE: 'N',
        },
        function(result) {
            if (result.error()) {
                console.error(result.error().ex);
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'imbot.message.delete',
        [
            'BOT_ID' => 39,
            'MESSAGE_ID' => 19880117,
            'COMPLETE' => 'N',
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Deleted: ' . ($result['result'] ? 'true' : 'false');
    }
    ```

{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": true,
    "time": {
        "start": 1728626400.123,
        "finish": 1728626400.234,
        "duration": 0.111,
        "processing": 0.045,
        "date_start": "2024-10-11T10:00:00+02:00",
        "date_finish": "2024-10-11T10:00:00+02:00",
        "operating_reset_at": 1762349466,
        "operating": 0
    }
}
```

## Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | `true` if the message was deleted without error. ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request. ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "CANT_EDIT_MESSAGE",
    "error_description": "Time has expired for modification or you don't have access"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `BOT_ID_ERROR` | Bot not found | The bot is not found or the application does not have an available bot for auto-completion of `BOT_ID`. ||
|| `APP_ID_ERROR` | Bot was installed by another REST application | The provided `BOT_ID` belongs to another application. ||
|| `MESSAGE_ID_ERROR` | Message ID can't be empty | A valid message identifier `MESSAGE_ID` was not provided. ||
|| `CANT_EDIT_MESSAGE` | Time has expired for modification or you don't have access | The time for deleting the message has expired or there is no access to the message. ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imbot-message-add.md)
- [{#T}](./imbot-message-update.md)
- [{#T}](./imbot-message-like.md)
- [{#T}](./events/on-imbot-message-delete.md)