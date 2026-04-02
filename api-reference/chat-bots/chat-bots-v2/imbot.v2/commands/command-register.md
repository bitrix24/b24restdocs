# Register the Team imbot.v2.Command.register

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the method: owner of the registered bot

The method `imbot.v2.Command.register` registers a bot's slash command.

This method is idempotent: a repeated call with the same `command` for the same bot from the same application returns the existing command without updating the data. To update, use [imbot.v2.Command.update](./command-update.md).

## Method Parameters

{% include [Parameter Notes](../../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **botId***
[`integer`](../../../../data-types.md) | Bot ID ||
|| **botToken**
[`string`](../../../../data-types.md) | Unique authorization token for the bot. Required for webhook authorization, not needed for OAuth.

Pass the same botToken that was specified during the chat bot registration ||
|| **command***
[`string`](../../../../data-types.md) | Command without the `/` symbol. For example: `help` ||
|| **title**
[`object`](../../../../data-types.md) | Title of the command in different languages. An object `{langCode: text}`, where `langCode` is the two-letter language code `en`, `ru`, `de`, etc. 
Displayed in the list of available commands. Required for visible commands `hidden: false`. For hidden commands `hidden: true`, it can be omitted ||
|| **params**
[`object`](../../../../data-types.md) | Description of command parameters in different languages. An object `{langCode: text}`, similar to `title`. Displayed as a hint next to the command ||
|| **common**
[`boolean`](../../../../data-types.md) | Common command. Acceptable values: `true`, `false`. Default is `false`. More details — [Common and Local Commands](#common-types) ||
|| **hidden**
[`boolean`](../../../../data-types.md) | Hidden command (not displayed in hints). Acceptable values: `true`, `false`. Default is `false` ||
|| **extranetSupport**
[`boolean`](../../../../data-types.md) | Extranet support. Acceptable values: `true`, `false`. Default is `false` ||
|#

## Common and Local Commands {#common-types}

The `common` parameter determines where the command is available:

- `common: true` — common command, available in any chat
- `common: false` — local command, available only in a personal dialogue with the bot and in chats where the bot is a participant

Typical use-cases:

- Common commands are suitable for global actions (e.g., search or help without the bot needing to be present in the chat)
- Local commands are suitable for actions tied to a specific bot and the context of its chat

The event for command invocation: [ONIMBOTV2COMMANDADD](../events/events.md#onimbotv2commandadd).

## Code Examples

{% include [Example Notes](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"botToken":"my_bot_token","command":"help","title":{"en":"Show help","ru":"Show help"}}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.v2.Command.register
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"command":"help","title":{"en":"Show help","ru":"Show help"},"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imbot.v2.Command.register
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.v2.Command.register', {
        botId: 456,
        command: 'help',
        title: { en: 'Show help', ru: 'Show help' },
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
                'imbot.v2.Command.register',
                [
                    'botId' => 456,
                    'command' => 'help',
                    'title' => ['en' => 'Show help', 'ru' => 'Show help'],
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
        'imbot.v2.Command.register',
        {
            botId: 456,
            command: 'help',
            title: { en: 'Show help', ru: 'Show help' },
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
        'imbot.v2.Command.register',
        [
            'botId' => 456,
            'command' => 'help',
            'title' => ['en' => 'Show help', 'ru' => 'Show help'],
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: '. $result['error_description'];
    } else {
        echo 'Command ID: '. $result['result']['command']['id'];
    }
    ```

{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": {
        "command": {
            "id": 42,
            "botId": 456,
            "command": "/help",
            "common": false,
            "hidden": false,
            "extranetSupport": false
        }
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
|| **result.command**
[`Command`](../../entities.md#command) | Object of the registered command [(detailed description)](#command-object) ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

### Fields of the Command Object {#command-object}

#|
|| **Field**
`Type` | **Description** ||
|| **id**
[`integer`](../../../../data-types.md) | Identifier of the command ||
|| **botId**
[`integer`](../../../../data-types.md) | Identifier of the bot ||
|| **command**
[`string`](../../../../data-types.md) | Text of the command ||
|| **common**
[`boolean`](../../../../data-types.md) | Command available in all chats ||
|| **hidden**
[`boolean`](../../../../data-types.md) | Command hidden from the command list ||
|| **extranetSupport**
[`boolean`](../../../../data-types.md) | Command available to extranet users ||
|#

A complete description of all object fields can be found on the [Objects and Fields](../../entities.md) page.

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "COMMAND_REQUIRED",
    "error_description": "Command is required"
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
|| `COMMAND_REQUIRED` | Command is required | Command (`command`) not specified ||
|| `COMMAND_TITLE_REQUIRED` | Command title is required | `title` not specified for visible command `hidden: false` ||
|| `COMMAND_REGISTER_FAILED` | Command registration failed | Error during command registration ||
|#

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [Update the imbot.v2.Command.update](./command-update.md)
- [Get the List of imbot.v2.Command.list Commands](./command-list.md)
- [Remove the imbot.v2.Command.unregister Command](./command-unregister.md)
- [Responding to the Command imbot.v2.Command.answer](./command-answer.md)
