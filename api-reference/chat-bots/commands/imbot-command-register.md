# Add the imbot.command.register Command

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: a user of the application that registered the chat bot

The method `imbot.command.register` registers a command for the chat bot.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **BOT_ID***
[`integer`](../../data-types.md) | The identifier of the chat bot. You can obtain the bot ID using the [imbot.bot.list](../imbot-bot-list.md) method ||
|| **COMMAND***
[`string`](../../data-types.md) | The text of the command that the user types in the chat. Latin letters and numbers can be used without spaces and special characters ||
|| **EVENT_COMMAND_ADD***
[`string`](../../data-types.md) | The URL of the event handler [ONIMCOMMANDADD](./events/on-im-command-add.md), which is called when the command is used ||
|| **LANG***
[`array`](../../data-types.md) | An array of localizations for the command. The structure is described [below](#lang) ||
|| **COMMON**
[`string`](../../data-types.md) | Command availability:
- `Y` - the command is available in any chats
- `N` - the command is only available where the bot is present

Default - `N` ||
|| **HIDDEN**
[`string`](../../data-types.md) | Command visibility:
- `Y` - hidden command
- `N` - visible command

Default - `N` ||
|| **EXTRANET_SUPPORT**
[`string`](../../data-types.md) | Command availability for extranet users:
- `Y` - available
- `N` - not available

Default - `N` ||
|| **CLIENT_ID**
[`string`](../../data-types.md) | This parameter is required only for webhooks. Pass the same CLIENT_ID that was specified when registering the chat bot ||
|#

### LANG Parameter {#lang}

#|
|| **Name**
`type` | **Description** ||
|| **LANGUAGE_ID***
[`string`](../../data-types.md) | Language identifier, e.g., `de` or `en` ||
|| **TITLE***
[`string`](../../data-types.md) | The name of the command in the selected language ||
|| **PARAMS**
[`string`](../../data-types.md) | A hint for the command parameters in the selected language ||
|#

{% note info "" %}

When registering multiple commands, specify the same URL in `EVENT_COMMAND_ADD`, and parse the specific command in the handler code by `COMMAND`/`COMMAND_ID`.

{% endnote %}

{% endnote %}

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"BOT_ID":1291,"COMMAND":"echo","EVENT_COMMAND_ADD":"https://example.com/bot/command.php","LANG":[{"LANGUAGE_ID":"de","TITLE":"Echo","PARAMS":"text"},{"LANGUAGE_ID":"en","TITLE":"Echo","PARAMS":"text"}],"COMMON":"Y","HIDDEN":"N","EXTRANET_SUPPORT":"N","CLIENT_ID":"**put_your_client_id_here**"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.command.register
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"BOT_ID":1291,"COMMAND":"echo","EVENT_COMMAND_ADD":"https://example.com/bot/command.php","LANG":[{"LANGUAGE_ID":"de","TITLE":"Echo","PARAMS":"text"},{"LANGUAGE_ID":"en","TITLE":"Echo","PARAMS":"text"}],"COMMON":"Y","HIDDEN":"N","EXTRANET_SUPPORT":"N","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/imbot.command.register
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'imbot.command.register',
            {
                BOT_ID: 1291,
                COMMAND: 'echo',
                EVENT_COMMAND_ADD: 'https://example.com/bot/command.php',
                LANG: [
                    { LANGUAGE_ID: 'de', TITLE: 'Echo', PARAMS: 'text' },
                    { LANGUAGE_ID: 'en', TITLE: 'Echo', PARAMS: 'text' }
                ],
                COMMON: 'Y',
                HIDDEN: 'N',
                EXTRANET_SUPPORT: 'N'
            }
        );
        
        const result = response.getData().result;
        console.log('Created element with ID:', result);
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
                'imbot.command.register',
                [
                    'BOT_ID' => 1291,
                    'COMMAND' => 'echo',
                    'EVENT_COMMAND_ADD' => 'https://example.com/bot/command.php',
                    'LANG' => [
                        ['LANGUAGE_ID' => 'de', 'TITLE' => 'Echo', 'PARAMS' => 'text'],
                        ['LANGUAGE_ID' => 'en', 'TITLE' => 'Echo', 'PARAMS' => 'text']
                    ],
                    'COMMON' => 'Y',
                    'HIDDEN' => 'N',
                    'EXTRANET_SUPPORT' => 'N'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding product row: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.command.register',
        {
            BOT_ID: 1291,
            COMMAND: 'echo',
            EVENT_COMMAND_ADD: 'https://example.com/bot/command.php',
            LANG: [
                { LANGUAGE_ID: 'de', TITLE: 'Echo', PARAMS: 'text' },
                { LANGUAGE_ID: 'en', TITLE: 'Echo', PARAMS: 'text' }
            ],
            COMMON: 'Y',
            HIDDEN: 'N',
            EXTRANET_SUPPORT: 'N'
        },
        function(result)
        {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'imbot.command.register',
        [
            'BOT_ID' => 1291,
            'COMMAND' => 'echo',
            'EVENT_COMMAND_ADD' => 'https://example.com/bot/command.php',
            'LANG' => [
                ['LANGUAGE_ID' => 'de', 'TITLE' => 'Echo', 'PARAMS' => 'text'],
                ['LANGUAGE_ID' => 'en', 'TITLE' => 'Echo', 'PARAMS' => 'text']
            ],
            'COMMON' => 'Y',
            'HIDDEN' => 'N',
            'EXTRANET_SUPPORT' => 'N'
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
    "result": 99,
    "time": {
        "start": 1772088116,
        "finish": 1772088116.785232,
        "duration": 0.7852320671081543,
        "processing": 0,
        "date_start": "2026-02-26T09:41:56+01:00",
        "date_finish": "2026-02-26T09:41:56+01:00",
        "operating_reset_at": 1772088716,
        "operating": 0.49926185607910156
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../data-types.md) | The identifier of the registered command ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "COMMAND_ERROR",
    "error_description": "Command isn't specified"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `EVENT_COMMAND_ADD_ERROR` | Handler for "Command add" event isn't specified | The `EVENT_COMMAND_ADD` parameter is missing ||
|| `EVENT_COMMAND_ADD_ERROR` | Wrong handler URL | An invalid URL for the command add event handler was provided ||
|| `COMMAND_ERROR` | Command isn't specified | The command text is not specified ||
|| `BOT_ID_ERROR` | Bot not found | The chat bot was not found ||
|| `APP_ID_ERROR` | Bot was installed by another REST application | The chat bot was registered by another application ||
|| `LANG_ERROR` | Lang set can't be empty | The `LANG` localization array is missing ||
|| `WRONG_REQUEST` | Command can't be created | Failed to register the command ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imbot-command-update.md)
- [{#T}](./imbot-command-unregister.md)
- [{#T}](./imbot-command-answer.md)
- [{#T}](./events/on-im-command-add.md)