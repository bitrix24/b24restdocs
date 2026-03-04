# Send a Response to the imbot.command.answer Command

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: a user of the application that registered the chatbot

The method `imbot.command.answer` publishes a response to a chatbot command.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **COMMAND_ID***
[`integer`](../../data-types.md) | Command identifier. Required if `COMMAND` is not provided ||
|| **COMMAND***
[`string`](../../data-types.md) | Text of the command. Required if `COMMAND_ID` is not provided ||
|| **MESSAGE_ID***
[`integer`](../../data-types.md) | Identifier of the message to which the response is sent.

The identifier can be obtained from the incoming event [ONIMCOMMANDADD](./events/on-im-command-add.md) ||
|| **MESSAGE***
[`string`](../../data-types.md) | Text of the response ||
|| **ATTACH**
[`object`](../../data-types.md) | Object with an attachment to the message. Minimum format: 

```json
{
  "BLOCKS": [
    { "MESSAGE": "Block text" }
  ]
}
```

[Detailed description](../../chats/messages/attachments/index.md) ||
|| **KEYBOARD**
[`object`](../../data-types.md) | Message keyboard. Minimum format:

```json
{
  "BUTTONS": [
    { "TEXT": "Repeat", "COMMAND": "echo repeat" }
  ]
}
```

[Detailed description](../../chats/messages/keyboards.md) ||
|| **MENU**
[`object`](../../data-types.md) | Context menu of the message. Minimum format:

```json
[
  { "TEXT": "bitrix24", "LINK": "https://bitrix24.com" }
]
```

[Detailed description](../../chats/messages/menu.md) ||
|| **SYSTEM**
[`string`](../../data-types.md) | Type of message:
- `Y` - system message
- `N` - regular message

Default is `N` ||
|| **URL_PREVIEW**
[`string`](../../data-types.md) | Link transformation into rich links:
- `Y` - enabled
- `N` - disabled

Default is `Y`.

Works for links provided in the `MESSAGE` field ||
|| **CLIENT_ID**
[`string`](../../data-types.md) | Technical parameter for scenarios without `clientId` in authorization.

If provided, used as `custom{CLIENT_ID}` to identify the application ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"COMMAND_ID":99,"MESSAGE_ID":33871,"MESSAGE":"Received. Executing command.","SYSTEM":"N","URL_PREVIEW":"Y","ATTACH":{"BLOCKS":[{"MESSAGE":"Task details"},{"DELIMITER":true},{"LINK":{"NAME":"Open","LINK":"https://example.com"}}]},"KEYBOARD":{"BUTTONS":[{"TEXT":"Repeat","COMMAND":"echo repeat"}]},"MENU":[{"TEXT":"bitrix24","LINK":"https://bitrix24.com"}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.command.answer
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"COMMAND_ID":99,"MESSAGE_ID":33871,"MESSAGE":"Received. Executing command.","SYSTEM":"N","URL_PREVIEW":"Y","ATTACH":{"BLOCKS":[{"MESSAGE":"Task details"},{"DELIMITER":true},{"LINK":{"NAME":"Open","LINK":"https://example.com"}}]},"KEYBOARD":{"BUTTONS":[{"TEXT":"Repeat","COMMAND":"echo repeat"}]},"MENU":[{"TEXT":"bitrix24","LINK":"https://bitrix24.com"}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/imbot.command.answer
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'imbot.command.answer',
            {
                COMMAND_ID: 99,
                MESSAGE_ID: 33871,
                MESSAGE: 'Received. Executing command.',
                SYSTEM: 'N',
                URL_PREVIEW: 'Y',
                ATTACH: {
                    BLOCKS: [
                        {MESSAGE: 'Task details'},
                        {DELIMITER: true},
                        {LINK: {NAME: 'Open', LINK: 'https://example.com'}}
                    ]
                },
                KEYBOARD: {
                    BUTTONS: [
                        {TEXT: 'Repeat', COMMAND: 'echo repeat'}
                    ]
                },
                MENU: [
                    {TEXT: 'bitrix24', LINK: 'https://bitrix24.com'}
                ]
            }
        );
        
        const result = response.getData().result;
        console.log('Answered command with ID:', result);
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
                'imbot.command.answer',
                [
                    'COMMAND_ID' => 99,
                    'MESSAGE_ID' => 33871,
                    'MESSAGE' => 'Received. Executing command.',
                    'SYSTEM' => 'N',
                    'URL_PREVIEW' => 'Y',
                    'ATTACH' => [
                        'BLOCKS' => [
                            ['MESSAGE' => 'Task details'],
                            ['DELIMITER' => true],
                            ['LINK' => ['NAME' => 'Open', 'LINK' => 'https://example.com']]
                        ]
                    ],
                    'KEYBOARD' => [
                        'BUTTONS' => [
                            ['TEXT' => 'Repeat', 'COMMAND' => 'echo repeat']
                        ]
                    ],
                    'MENU' => [
                        ['TEXT' => 'bitrix24', 'LINK' => 'https://bitrix24.com']
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
        echo 'Error answering command: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.command.answer',
        {
            COMMAND_ID: 99,
            MESSAGE_ID: 33871,
            MESSAGE: 'Received. Executing command.',
            SYSTEM: 'N',
            URL_PREVIEW: 'Y',
            ATTACH: {
                BLOCKS: [
                    {MESSAGE: 'Task details'},
                    {DELIMITER: true},
                    {LINK: {NAME: 'Open', LINK: 'https://example.com'}}
                ]
            },
            KEYBOARD: {
                BUTTONS: [
                    {TEXT: 'Repeat', COMMAND: 'echo repeat'}
                ]
            },
            MENU: [
                    {TEXT: 'bitrix24', LINK: 'https://bitrix24.com'}
            ]
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
        'imbot.command.answer',
        [
            'COMMAND_ID' => 99,
            'MESSAGE_ID' => 33871,
            'MESSAGE' => 'Received. Executing command.',
            'SYSTEM' => 'N',
            'URL_PREVIEW' => 'Y',
            'ATTACH' => [
                'BLOCKS' => [
                    ['MESSAGE' => 'Task details'],
                    ['DELIMITER' => true],
                    ['LINK' => ['NAME' => 'Open', 'LINK' => 'https://example.com']]
                ]
            ],
            'KEYBOARD' => [
                'BUTTONS' => [
                    ['TEXT' => 'Repeat', 'COMMAND' => 'echo repeat']
                ]
            ],
            'MENU' => [
                ['TEXT' => 'bitrix24', 'LINK' => 'https://bitrix24.com']
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
    "result": 33879,
    "time": {
        "start": 1772102358,
        "finish": 1772102359.061859,
        "duration": 1.061858892440796,
        "processing": 1,
        "date_start": "2026-02-26T13:39:18+01:00",
        "date_finish": "2026-02-26T13:39:19+01:00",
        "operating_reset_at": 1772102958,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../data-types.md) | Identifier of the sent response message ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "MESSAGE_EMPTY",
    "error_description": "Message can't be empty"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `COMMAND_ID_ERROR` | Command not found | Command not found ||
|| `APP_ID_ERROR` | Command was installed by another REST application | Command registered by another application ||
|| `MESSAGE_ID_EMPTY` | Message ID can't be empty | `MESSAGE_ID` not provided ||
|| `MESSAGE_EMPTY` | Message can't be empty | Message text not provided ||
|| `ATTACH_ERROR` | Incorrect attach params | Invalid `ATTACH` object ||
|| `ATTACH_OVERSIZE` | You have exceeded the maximum allowable size of attach | `ATTACH` size exceeds the allowable limit ||
|| `KEYBOARD_ERROR` | Incorrect keyboard params | Invalid `KEYBOARD` object ||
|| `KEYBOARD_OVERSIZE` | You have exceeded the maximum allowable size of keyboard | `KEYBOARD` size exceeds the allowable limit ||
|| `MENU_ERROR` | Incorrect menu params | Invalid `MENU` object ||
|| `WRONG_REQUEST` | Message isn't added | Failed to send message ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imbot-command-register.md)
- [{#T}](./imbot-command-update.md)
- [{#T}](./imbot-command-unregister.md)
- [{#T}](./events/on-im-command-add.md)
- [{#T}](../../chats/messages/keyboards.md)
- [{#T}](../../chats/messages/attachments/index.md)
- [{#T}](../../chats/messages/menu.md)