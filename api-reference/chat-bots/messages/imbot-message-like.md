# Set "Like" for imbot.message.like

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: an authorized user of the application that registered the chatbot. The method only works with bots from this application.

The method `imbot.message.like` sets or removes a "Like" mark for a message.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **BOT_ID**
[`integer`](../../data-types.md) | The identifier of the chatbot. You can obtain the bot ID using the [imbot.bot.list](../imbot-bot-list.md) method.

If the parameter is not provided, the method searches for the first bot registered by the current application. ||
|| **MESSAGE_ID*** 
[`integer`](../../data-types.md) | The identifier of the message in personal dialogs or group chats where the chatbot is present. The value must be greater than `0`.

For messages sent by the bot via REST, the identifier is returned by the [imbot.message.add](./imbot-message-add.md) method.

Message IDs sent to the chat by users can be obtained using the [im.dialog.messages.get](../../chats/messages/im-dialog-messages-get.md) method. ||
|| **ACTION**
[`string`](../../data-types.md) | The action for reacting to the message.

Allowed values:
- `auto` — automatically toggle the current status: if there is no "Like," it will be set; if there is a "Like," it will be removed.
- `plus` — set "Like."
- `minus` — remove "Like."

If the parameter is not provided, `auto` is used. ||
|| **CLIENT_ID**
[`string`](../../data-types.md) | A technical parameter for scenarios without `clientId` in authorization.

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
      -d '{"BOT_ID":39,"MESSAGE_ID":19880117,"ACTION":"auto"}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.message.like
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"BOT_ID":39,"MESSAGE_ID":19880117,"ACTION":"auto","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imbot.message.like
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.message.like', {
        BOT_ID: 39,
        MESSAGE_ID: 19880117,
        ACTION: 'plus',
      });

      const { result } = response.getData();
      console.log('Like status changed:', result);
    } catch (error) {
      console.error('Error changing status:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'imbot.message.like',
                [
                    'BOT_ID' => 39,
                    'MESSAGE_ID' => 19880117,
                    'ACTION' => 'auto',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Like status changed: ' . ($result->data() ? 'true' : 'false');
        }
    } catch (Throwable $exception) {
        error_log($exception->getMessage());
        echo 'Error changing status: ' . $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.message.like',
        {
            BOT_ID: 39,
            MESSAGE_ID: 19880117,
            ACTION: 'auto',
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
        'imbot.message.like',
        [
            'BOT_ID' => 39,
            'MESSAGE_ID' => 19880117,
            'ACTION' => 'auto',
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Like status changed: ' . ($result['result'] ? 'true' : 'false');
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
[`boolean`](../../data-types.md) | `true` if the action was executed successfully. ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time. ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "WITHOUT_CHANGES",
    "error_description": "Action completed without changes"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `BOT_ID_ERROR` | Bot not found | The bot was not found or the application does not have an available bot for auto-filling `BOT_ID`. ||
|| `APP_ID_ERROR` | Bot was installed by another REST application | The provided `BOT_ID` belongs to another application. ||
|| `MESSAGE_ID_ERROR` | Message ID can't be empty | The message identifier was not provided. ||
|| `WITHOUT_CHANGES` | Action completed without changes | The reaction state did not change after the call. ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imbot-message-add.md)
- [{#T}](./imbot-message-update.md)
- [{#T}](./imbot-message-delete.md)