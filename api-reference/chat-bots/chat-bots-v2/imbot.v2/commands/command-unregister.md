# Remove the imbot.v2.Command.unregister Command

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the method: owner of the registered bot

The method `imbot.v2.Command.unregister` removes a bot's slash command.

## Method Parameters

{% include [Parameters Note](../../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **botId***
[`integer`](../../../../data-types.md) | Bot ID ||
|| **botToken**
[`string`](../../../../data-types.md) | Unique authorization token for the bot. Required for webhook authorization, not needed for OAuth.

Pass the same botToken that was specified during the chat bot registration ||
|| **commandId***
[`integer`](../../../../data-types.md) | Command ID ||
|#

## Code Examples

{% include [Examples Note](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"botToken":"my_bot_token","commandId":42}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.v2.Command.unregister
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"commandId":42,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imbot.v2.Command.unregister
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.v2.Command.unregister', {
        botId: 456,
        commandId: 42,
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
                'imbot.v2.Command.unregister',
                [
                    'botId' => 456,
                    'commandId' => 42,
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
        'imbot.v2.Command.unregister',
        {
            botId: 456,
            commandId: 42,
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
        'imbot.v2.Command.unregister',
        [
            'botId' => 456,
            'commandId' => 42,
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Command deleted';
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
[`boolean`](../../../../data-types.md) | `true` if deletion is successful ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "COMMAND_NOT_FOUND",
    "error_description": "Command not found"
}
```

{% include notitle [Error Handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `BOT_TOKEN_NOT_SPECIFIED` | Bot token is not specified | `botToken` is not provided. Required for webhook authorization ||
|| `BOT_ID_REQUIRED` | Bot ID is required | `botId` is not provided ||
|| `BOT_NOT_FOUND` | Bot not found | Bot not found ||
|| `BOT_OWNERSHIP_ERROR` | Bot is registered by another application | Bot is registered by another application ||
|| `COMMAND_NOT_FOUND` | Command not found | Command not found or access is denied ||
|#

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [Register the Team imbot.v2.Command.register](./command-register.md)
- [Get the List of imbot.v2.Command.list Commands](./command-list.md)