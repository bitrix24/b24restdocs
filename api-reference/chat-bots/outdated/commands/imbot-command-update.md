# Update the imbot.command.update

> Scope: [`imbot`](../../../scopes/permissions.md)
>
> Who can execute the method: a user of the application that registered the chat bot

{% note warning "DEPRECATED" %}

Development of this method has been halted. Please use [imbot.v2.Command.update](../../chat-bots-v2/imbot.v2/commands/command-update.md).

{% endnote %}

The method `imbot.command.update` updates the parameters of a registered chat bot command.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **COMMAND_ID***
[`integer`](../../../data-types.md) | Identifier of the command to be updated ||
|| **FIELDS***
[`object`](../../../data-types.md) | Object containing fields to update. The structure is described [below](#fields) ||
|| **CLIENT_ID**
[`string`](../../../data-types.md) | This parameter is required only for webhooks. Pass the same CLIENT_ID that was specified during the registration of the chat bot ||
|#

{% note warning "" %}

At least one modifiable parameter must be provided in `FIELDS`. If an empty object is passed, the method will return an error.

{% endnote %}

### FIELDS Parameter {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **COMMAND**
[`string`](../../../data-types.md) | Command text ||
|| **EVENT_COMMAND_ADD**
[`string`](../../../data-types.md) | URL of the event handler [ONIMCOMMANDADD](./events/on-im-command-add.md) ||
|| **HIDDEN**
[`string`](../../../data-types.md) | Command visibility:
- `Y` - hidden
- `N` - visible ||
|| **EXTRANET_SUPPORT**
[`string`](../../../data-types.md) | Availability for extranet users:
- `Y` - available
- `N` - not available ||
|| **LANG**
[`array`](../../../data-types.md) | Array of command localizations. The structure is described [below](#fields-lang) ||
|#

### FIELDS.LANG Parameter {#fields-lang}

#|
|| **Name**
`type` | **Description** ||
|| **LANGUAGE_ID***
[`string`](../../../data-types.md) | Language identifier, e.g., `de` or `en` ||
|| **TITLE***
[`string`](../../../data-types.md) | Command title in the selected language ||
|| **PARAMS**
[`string`](../../../data-types.md) | Parameter hint for the command in the selected language ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST /
    -H "Content-Type: application/json" /
    -H "Accept: application/json" /
    -d '{"COMMAND_ID":99,"FIELDS":{"COMMAND":"echo2","EVENT_COMMAND_ADD":"https://example.com/bot/command.php","HIDDEN":"N","EXTRANET_SUPPORT":"Y","LANG":[{"LANGUAGE_ID":"de","TITLE":"Echo 2","PARAMS":"text"},{"LANGUAGE_ID":"en","TITLE":"Echo 2","PARAMS":"text"}]},"CLIENT_ID":"**put_your_client_id_here**"}' /
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.command.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST /
    -H "Content-Type: application/json" /
    -H "Accept: application/json" /
    -d '{"COMMAND_ID":99,"FIELDS":{"COMMAND":"echo2","EVENT_COMMAND_ADD":"https://example.com/bot/command.php","HIDDEN":"N","EXTRANET_SUPPORT":"Y","LANG":[{"LANGUAGE_ID":"de","TITLE":"Echo 2","PARAMS":"text"},{"LANGUAGE_ID":"en","TITLE":"Echo 2","PARAMS":"text"}]},"auth":"**put_access_token_here**"}' /
    https://**put_your_bitrix24_address**/rest/imbot.command.update
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'imbot.command.update',
            {
                COMMAND_ID: 99,
                FIELDS: {
                    COMMAND: 'echo2',
                    EVENT_COMMAND_ADD: 'https://example.com/bot/command.php',
                    HIDDEN: 'N',
                    EXTRANET_SUPPORT: 'Y',
                    LANG: [
                        { LANGUAGE_ID: 'de', TITLE: 'Echo 2', PARAMS: 'text' },
                        { LANGUAGE_ID: 'en', TITLE: 'Echo 2', PARAMS: 'text' }
                    ]
                }
            }
        );
        
        const result = response.getData().result;
        console.log('Updated command with ID:', result);
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
                'imbot.command.update',
                [
                    'COMMAND_ID' => 99,
                    'FIELDS' => [
                        'COMMAND' => 'echo2',
                        'EVENT_COMMAND_ADD' => 'https://example.com/bot/command.php',
                        'HIDDEN' => 'N',
                        'EXTRANET_SUPPORT' => 'Y',
                        'LANG' => [
                            ['LANGUAGE_ID' => 'de', 'TITLE' => 'Echo 2', 'PARAMS' => 'text'],
                            ['LANGUAGE_ID' => 'en', 'TITLE' => 'Echo 2', 'PARAMS' => 'text']
                        ]
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating command: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.command.update',
        {
            COMMAND_ID: 99,
            FIELDS: {
                COMMAND: 'echo2',
                EVENT_COMMAND_ADD: 'https://example.com/bot/command.php',
                HIDDEN: 'N',
                EXTRANET_SUPPORT: 'Y',
                LANG: [
                    { LANGUAGE_ID: 'de', TITLE: 'Echo 2', PARAMS: 'text' },
                    { LANGUAGE_ID: 'en', TITLE: 'Echo 2', PARAMS: 'text' }
                ]
            }
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
        'imbot.command.update',
        [
            'COMMAND_ID' => 99,
            'FIELDS' => [
                'COMMAND' => 'echo2',
                'EVENT_COMMAND_ADD' => 'https://example.com/bot/command.php',
                'HIDDEN' => 'N',
                'EXTRANET_SUPPORT' => 'Y',
                'LANG' => [
                    ['LANGUAGE_ID' => 'de', 'TITLE' => 'Echo 2', 'PARAMS' => 'text'],
                    ['LANGUAGE_ID' => 'en', 'TITLE' => 'Echo 2', 'PARAMS' => 'text']
                ]
            ]
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
        "start": 1772103002,
        "finish": 1772103003.109342,
        "duration": 1.109342098236084,
        "processing": 0,
        "date_start": "2026-02-26T13:50:02+01:00",
        "date_finish": "2026-02-26T13:50:03+01:00",
        "operating_reset_at": 1772103603,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | `true` if the command was successfully updated ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "WRONG_REQUEST",
    "error_description": "Update fields can't be empty"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `COMMAND_ID_ERROR` | Command not found | Command not found ||
|| `APP_ID_ERROR` | Command was installed by another rest application | Command registered by another application ||
|| `EVENT_COMMAND_ADD_ERROR` | Wrong handler URL | Invalid event handler URL ||
|| `WRONG_REQUEST` | Update fields can't be empty | No fields provided for update ||
|| `WRONG_REQUEST` | Command can't be updated | Failed to update command ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [Add the imbot.command.register Command](./imbot-command-register.md)
- [Send a response to the command imbot.command.answer](./imbot-command-answer.md)
- [Remove the imbot.command.unregister Command](./imbot-command-unregister.md)
- [Event Triggered by Chat Bot Command ONIMCOMMANDADD](./events/on-im-command-add.md)
