# Execute the chat-bot command im.message.command

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: chat participant

The method `im.message.command` executes a chat-bot command in the context of a message.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **MESSAGE_ID***
[`integer`](../../data-types.md) | The identifier of the message in the context of which the command is executed.

The identifier can be obtained using the method [im.dialog.messages.get](./im-dialog-messages-get.md) ||
|| **BOT_ID***
[`integer`](../../data-types.md) | The identifier of the bot.

The identifier can be obtained using the method [imbot.bot.list](../../chat-bots/outdated/bots/imbot-bot-list.md). ||
|| **COMMAND***
[`string`](../../data-types.md) | The bot command.

The text is specified when registering the command by the chat-bot through the method [imbot.command.register](../../chat-bots/outdated/commands/imbot-command-register.md) ||
|| **COMMAND_PARAMS**
[`string`](../../data-types.md) | Command parameter ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"MESSAGE_ID":34261,"BOT_ID":1291,"COMMAND":"task","COMMAND_PARAMS":"task #1"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.message.command
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"MESSAGE_ID":34261,"BOT_ID":1291,"COMMAND":"task","COMMAND_PARAMS":"task #1","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/im.message.command
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'im.message.command',
            {
                MESSAGE_ID: 34261,
                BOT_ID: 1291,
                COMMAND: 'task',
                COMMAND_PARAMS: 'task #1'
            }
        );
        
        const result = response.getData().result;
        console.log('Executed command with result:', result);
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
                'im.message.command',
                [
                    'MESSAGE_ID' => 34261,
                    'BOT_ID' => 1291,
                    'COMMAND' => 'task',
                    'COMMAND_PARAMS' => 'task #1'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error executing command: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.message.command',
        {
            MESSAGE_ID: 34261,
            BOT_ID: 1291,
            COMMAND: 'task',
            COMMAND_PARAMS: 'task #1'
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
        'im.message.command',
        [
            'MESSAGE_ID' => 34261,
            'BOT_ID' => 1291,
            'COMMAND' => 'task',
            'COMMAND_PARAMS' => 'task #1'
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
        "start": 1772633574,
        "finish": 1772633574.201127,
        "duration": 0.2011270523071289,
        "processing": 0,
        "date_start": "2026-03-04T17:12:54+01:00",
        "date_finish": "2026-03-04T17:12:54+01:00",
        "operating_reset_at": 1772634174,
        "operating": 0.10915994644165039
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | `true` if the command was executed successfully ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "PARAMS_ERROR",
    "error_description": "Incorrect params"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `MESSAGE_ID_ERROR` | Message ID can't be empty | `MESSAGE_ID` is missing or incorrect ||
|| `BOT_ID_ERROR` | Bot ID can't be empty | `BOT_ID` is missing or incorrect ||
|| `COMMAND_ERROR` | Command can't be empty | `COMMAND` is missing ||
|| `PARAMS_ERROR` | Incorrect params | Command parameters are incorrect ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-message-add.md)
- [{#T}](./keyboards.md)
- [{#T}](./menu.md)