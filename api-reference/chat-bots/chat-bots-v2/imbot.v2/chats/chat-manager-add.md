# Add Chat Managers imbot.v2.Chat.Manager.add

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the method: owner of the registered bot

The method `imbot.v2.Chat.Manager.add` adds users to the current list of chat managers. The bot must be the owner of the chat.

{% note info "" %}

Users who are not participants in the chat will be ignored without an error.

{% endnote %}

## Method Parameters

{% include [Footnote about parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **botId***
[`integer`](../../../../data-types.md) | Bot ID ||
|| **botToken**
[`string`](../../../../data-types.md) | Unique authorization token for the bot. Required for webhook authorization, not needed for OAuth.

Pass the same botToken that was specified during the chat bot registration ||
|| **dialogId***
[`string`](../../../../data-types.md) | Dialog ID. For group chats — `chat{chatId}` ||
|| **userIds***
[`integer[]`](../../../../data-types.md) | Array of user IDs to assign as managers ||
|#

## Code Examples

{% include [Footnote about examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"botToken":"my_bot_token","dialogId":"chat5","userIds":[4,5]}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.v2.Chat.Manager.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"dialogId":"chat5","userIds":[4,5],"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imbot.v2.Chat.Manager.add
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.v2.Chat.Manager.add', {
        botId: 456,
        dialogId: 'chat5',
        userIds: [4, 5],
      });

      const { result } = response.getData();
      console.log('result:', result);
    } catch (error) {
      console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'imbot.v2.Chat.Manager.add',
                [
                    'botId' => 456,
                    'dialogId' => 'chat5',
                    'userIds' => [4, 5],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'result: ' . print_r($result, true);
    } catch (Throwable $exception) {
        error_log($exception->getMessage());
        echo 'Error: ' . $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.v2.Chat.Manager.add',
        {
            botId: 456,
            dialogId: 'chat5',
            userIds: [4, 5],
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
        'imbot.v2.Chat.Manager.add',
        [
            'botId' => 456,
            'dialogId' => 'chat5',
            'userIds' => [4, 5],
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Managers added';
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
        "date_finish": "2024-10-11T10:00:00+02:00"
    }
}
```

## Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`boolean`](../../../../data-types.md) | `true` when managers are successfully added ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Access denied"
}
```

{% include notitle [Error Handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `BOT_TOKEN_NOT_SPECIFIED` | Bot token is not specified | `botToken` is not provided. Required for webhook authorization ||
|| `BOT_ID_REQUIRED` | Bot ID is required | `botId` is not provided ||
|| `EMPTY_USER_IDS` | Empty user IDs | The `userIds` array is not provided or is empty ||
|| `BOT_NOT_FOUND` | Bot not found | Bot not found ||
|| `BOT_OWNERSHIP_ERROR` | Bot is registered by another application | Bot is registered by another application ||
|| `ACCESS_DENIED` | Access denied | The bot is not a participant in the chat or lacks permissions (owner role required) ||
|#

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [Remove Chat Managers imbot.v2.Chat.Manager.delete](./chat-manager-delete.md)
- [Assign Chat Owner imbot.v2.Chat.setOwner](./chat-set-owner.md)