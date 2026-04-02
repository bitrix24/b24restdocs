# Remove the imbot.command.unregister Command

> Scope: [`imbot`](../../../scopes/permissions.md)
>
> Who can execute the method: a user of the application that registered the chat bot

{% note warning "DEPRECATED" %}

Development of this method has been halted. Please use [imbot.v2.Command.unregister](../../chat-bots-v2/imbot.v2/commands/command-unregister.md).

{% endnote %}

The method `imbot.command.unregister` removes a registered command from the chat bot.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **COMMAND_ID***
[`integer`](../../../data-types.md) | Identifier of the command to be removed ||
|| **CLIENT_ID**
[`string`](../../../data-types.md) | This parameter is required only for webhooks. Pass the same CLIENT_ID that was specified during the registration of the chat bot ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST /
    -H "Content-Type: application/json" /
    -H "Accept: application/json" /
    -d '{"COMMAND_ID":99,"CLIENT_ID":"**put_your_client_id_here**"}' /
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.command.unregister
    ```

- cURL (OAuth)

    ```bash
    curl -X POST /
    -H "Content-Type: application/json" /
    -H "Accept: application/json" /
    -d '{"COMMAND_ID":99,"auth":"**put_access_token_here**"}' /
    https://**put_your_bitrix24_address**/rest/imbot.command.unregister
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'imbot.command.unregister',
            {
                COMMAND_ID: 99
            }
        );
        
        const result = response.getData().result;
        console.log('Unregistered command with ID:', result);
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
                'imbot.command.unregister',
                [
                    'COMMAND_ID' => 99
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error unregistering command: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.command.unregister',
        {
            COMMAND_ID: 99
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
        'imbot.command.unregister',
        [
            'COMMAND_ID' => 99
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
        "start": 1772105363,
        "finish": 1772105363.56206,
        "duration": 0.5620601177215576,
        "processing": 0,
        "date_start": "2026-02-26T14:29:23+01:00",
        "date_finish": "2026-02-26T14:29:23+01:00",
        "operating_reset_at": 1772105963,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | `true` if the command was successfully removed ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "COMMAND_ID_ERROR",
    "error_description": "Command not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `COMMAND_ID_ERROR` | Command not found | Command not found ||
|| `APP_ID_ERROR` | Command was installed by another REST application | Command registered by another application ||
|| `WRONG_REQUEST` | Command can't be deleted | Failed to delete command ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [Add the imbot.command.register Command](./imbot-command-register.md)
- [Update the imbot.command.update](./imbot-command-update.md)
- [Send a response to the command imbot.command.answer](./imbot-command-answer.md)
- [Event Triggered by Chat Bot Command ONIMCOMMANDADD](./events/on-im-command-add.md)
