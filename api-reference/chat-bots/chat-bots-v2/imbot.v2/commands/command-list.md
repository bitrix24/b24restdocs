# Get the List of imbot.v2.Command.list Commands

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the method: owner of the registered bot

The method `imbot.v2.Command.list` returns a list of all commands of the bot.

## Method Parameters

{% include [Footnote on parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **botId***
[`integer`](../../../../data-types.md) | Bot ID ||
|| **botToken**
[`string`](../../../../data-types.md) | Unique authorization token for the bot. Required for webhook authorization, not needed for OAuth.

Pass the same botToken that was specified during the chat-bot registration ||
|#

## Code Examples

{% include [Footnote on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"botToken":"my_bot_token"}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.v2.Command.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imbot.v2.Command.list
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.v2.Command.list', {
        botId: 456,
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
                'imbot.v2.Command.list',
                [
                    'botId' => 456,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'result: '. print_r($result, true);
    } catch (Throwable $exception) {
        error_log($exception->getMessage());
        echo 'Error: '. $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.v2.Command.list',
        {
            botId: 456,
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
        'imbot.v2.Command.list',
        [
            'botId' => 456,
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: '. $result['error_description'];
    } else {
        print_r($result['result']['commands']);
    }
    ```

{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": {
        "commands": [
            {
                "id": 42,
                "botId": 456,
                "command": "/help",
                "title": "Show Help",
                "params": "request text",
                "common": false,
                "hidden": false,
                "extranetSupport": false,
                "category": "My Bot",
                "context": ""
            }
        ]
    },
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
[`object`](../../../../data-types.md) | Result of the operation ||
|| **result.commands**
[`Command[]`](../../entities.md#command) | Array of bot commands [(detailed description)](#command-object) ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

### Fields of the Command Object {#command-object}

#|
|| **Field**
`Type` | **Description** ||
|| **id**
[`integer`](../../../../data-types.md) | Command identifier ||
|| **botId**
[`integer`](../../../../data-types.md) | Bot identifier ||
|| **command**
[`string`](../../../../data-types.md) | Command text ||
|| **title**
[`object`](../../../../data-types.md) | Localized titles of the command ||
|| **params**
[`object`](../../../../data-types.md) | Localized descriptions of parameters ||
|| **common**
[`boolean`](../../../../data-types.md) | Command available in all chats ||
|| **hidden**
[`boolean`](../../../../data-types.md) | Command hidden from the command list ||
|| **extranetSupport**
[`boolean`](../../../../data-types.md) | Command available to extranet users ||
|| **category**
[`string`](../../../../data-types.md) | Command category ||
|| **context**
[`array`](../../../../data-types.md) | Contexts of command usage ||
|#

Complete description of all object fields can be found on the [Objects and Fields](../../entities.md) page.

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "BOT_NOT_FOUND",
    "error_description": "Bot not found"
}
```

{% include notitle [Error Handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `BOT_TOKEN_NOT_SPECIFIED` | Bot token is not specified | `botToken` not specified. Required for webhook authorization ||
|| `BOT_ID_REQUIRED` | Bot ID is required | `botId` not specified ||
|| `BOT_NOT_FOUND` | Bot not found | Bot not found ||
|| `BOT_OWNERSHIP_ERROR` | Bot is registered by another application | Bot registered by another application ||
|#

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [Register the Team imbot.v2.Command.register](./command-register.md)
- [Update the imbot.v2.Command.update](./command-update.md)
- [Remove the imbot.v2.Command.unregister Command](./command-unregister.md)
- [Responding to the Command imbot.v2.Command.answer](./command-answer.md)