# Send the bot action indicator imbot.v2.Chat.InputAction.notify

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the method: owner of the registered bot

The method `imbot.v2.Chat.InputAction.notify` sends a bot action indicator to the chat — for example, "bot is typing".

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
[`string`](../../../../data-types.md) | Dialog ID. For group chats — `chat{chatId}`, for personal chats — `{userId}` ||
|| **statusMessageCode**
[`string`](../../../../data-types.md) | Action status code. A list of available codes is described [below](#status-codes). If not specified, the standard "typing" indicator is displayed ||
|| **duration**
[`integer`](../../../../data-types.md) | Duration of the indicator display in seconds (1–600). Default is determined by the server ||
|#

### Available statusMessageCode {#status-codes}

#|
|| **Code** | **Displayed Text** ||
|| `IMBOT_AGENT_ACTION_THINKING` | Agent is thinking... ||
|| `IMBOT_AGENT_ACTION_SEARCHING` | Agent is searching for information... ||
|| `IMBOT_AGENT_ACTION_GENERATING` | Agent is preparing a response... ||
|| `IMBOT_AGENT_ACTION_ANALYZING` | Agent is analyzing the request... ||
|| `IMBOT_AGENT_ACTION_PROCESSING` | Agent is processing data... ||
|| `IMBOT_AGENT_ACTION_TRANSLATING` | Agent is translating text... ||
|| `IMBOT_AGENT_ACTION_CONNECTING` | Agent is connecting to the service... ||
|| `IMBOT_AGENT_ACTION_CHECKING` | Agent is checking data... ||
|| `IMBOT_AGENT_ACTION_CALCULATING` | Agent is calculating... ||
|| `IMBOT_AGENT_ACTION_READING_DOCS` | Agent is reviewing documents... ||
|| `IMBOT_AGENT_ACTION_COMPOSING` | Agent is composing a response... ||
|#

## Code Examples

{% include [Footnote about examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"botToken":"my_bot_token","dialogId":"chat5","statusMessageCode":"IMBOT_AGENT_ACTION_THINKING","duration":30}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.v2.Chat.InputAction.notify
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"dialogId":"chat5","statusMessageCode":"IMBOT_AGENT_ACTION_THINKING","duration":30,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imbot.v2.Chat.InputAction.notify
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.v2.Chat.InputAction.notify', {
        botId: 456,
        dialogId: 'chat5',
        statusMessageCode: 'IMBOT_AGENT_ACTION_THINKING',
        duration: 30,
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
                'imbot.v2.Chat.InputAction.notify',
                [
                    'botId' => 456,
                    'dialogId' => 'chat5',
                    'statusMessageCode' => 'IMBOT_AGENT_ACTION_THINKING',
                    'duration' => 30,
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
        'imbot.v2.Chat.InputAction.notify',
        {
            botId: 456,
            dialogId: 'chat5',
            statusMessageCode: 'IMBOT_AGENT_ACTION_THINKING',
            duration: 30,
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
        'imbot.v2.Chat.InputAction.notify',
        [
            'botId' => 456,
            'dialogId' => 'chat5',
            'statusMessageCode' => 'IMBOT_AGENT_ACTION_THINKING',
            'duration' => 30,
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Action sent';
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
[`boolean`](../../../../data-types.md) | `true` if the indicator was successfully sent ||
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
|| `BOT_TOKEN_NOT_SPECIFIED` | Bot token is not specified | `botToken` is required for webhook authorization ||
|| `BOT_ID_REQUIRED` | Bot ID is required | `botId` is not specified ||
|| `BOT_NOT_FOUND` | Bot not found | Bot not found ||
|| `BOT_OWNERSHIP_ERROR` | Bot is registered by another application | Bot is registered by another application ||
|| `ACCESS_DENIED` | Access denied | Bot is not a participant in the chat ||
|#

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [API Change Log for imbot.v2](../../change-log.md)
- [{#T}](./chat-text-field-enabled.md)
- [{#T}](../messages/chat-message-send.md)