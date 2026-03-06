# Working with Keyboards
A keyboard consists of buttons beneath the message. They can be used to open links, perform actions, and trigger commands.

Methods that support keyboard functionality:

- [imbot.message.add](../../chat-bots/messages/imbot-message-add.md) — send a message on behalf of the chat bot
- [imbot.message.update](../../chat-bots/messages/imbot-message-update.md) — modify a sent message from the chat bot
- [imbot.command.answer](../../chat-bots/commands/imbot-command-answer.md) — send a response to a chat bot command
- [im.message.add](./im-message-add.md) — send a message in the chat
- [im.message.update](./im-message-update.md) — modify a sent message

## How to Add a Keyboard

To add a keyboard, pass the `KEYBOARD` parameter when creating or updating a message.

`KEYBOARD` can be passed as:

- a JSON string
- an object with the root key `BUTTONS`
- an array of buttons without wrapping

If the `KEYBOARD` does not contain the `BUTTONS` key, the server will automatically assume that a shortened format has been provided and will wrap the array in `BUTTONS`.

{% list tabs %}

- Full format with the BUTTONS key

  ```json
  {
      "KEYBOARD": {
          "BUTTONS": [
          { "TEXT": "Button", "LINK": "https://example.com" }
          ]
      }
  }
  ```

- Shortened format

  ```json
  {
      "KEYBOARD": [
          { "TEXT": "Button", "LINK": "https://example.com" }
          ]
  }
  ```

{% endlist %}

## Button Fields

#|
|| **Name**
`type` | **Description** ||
|| **TEXT**
[`string`](../../data-types.md) | Button text.

For all buttons except `TYPE`, it is mandatory to specify `TEXT` and one action field — `LINK`, `COMMAND`, `ACTION + ACTION_VALUE`, or `APP_ID` ||
|| **TYPE**
[`string`](../../data-types.md) | Move the button to a new line. The only allowed value is `NEWLINE` ||
|| **LINK**
[`string`](../../data-types.md) | Button link. `http/https` and relative path `/...` are allowed ||
|| **APP_ID**
[`integer`](../../data-types.md) | Application identifier for the chat.

Deprecated scenario. To open an application from the chat, use widgets ||
|| **APP_PARAMS**
[`string`](../../data-types.md) | Parameters for launching the application in the chat. Pass together with `APP_ID`.

Deprecated scenario. To open an application from the chat, use widgets

{% note info "" %}

Currently, the option with parameters `APP_ID` and `APP_PARAMS` is used in chats [Open Channels](../../imopenlines/openlines/index.md)

{% endnote %}
||
|| **ACTION**
[`string`](../../data-types.md) | Action:

- `PUT` — insert text into the input field 
- `SEND` — send text 
- `COPY` — copy text to the clipboard
- `CALL` — make a call
- `DIALOG` — open chat

Available starting from [REST API IM revision](../im-revision-get.md) 28 ||
|| **ACTION_VALUE**
[`string`](../../data-types.md) | Value for `ACTION`:

- `PUT` — text to be inserted into the input field
- `SEND` — text to be sent
- `COPY` — text to be copied to the clipboard
- `CALL` — phone number in international format
- `DIALOG` — chat identifier in the format `chatXXX` for group chat and `ID` of the user for personal chat 

Available starting from [REST API IM revision](../im-revision-get.md) 28 ||
|| **COMMAND**
[`string`](../../data-types.md) | Command for the bot.

Read more about command processing by the chat bot [below](#command-processing) ||
|| **COMMAND_PARAMS**
[`string`](../../data-types.md) | Command parameters. Pass together with `COMMAND` ||
|| **BLOCK**
[`string`](../../data-types.md) | Button blocking.

Allowed values:
- `Y` — block the button after pressing
- `N` — do not block the button after pressing 

Default — `N` ||
|| **DISABLED**
[`string`](../../data-types.md) | Button activity.

Allowed values:
- `Y` — button is inactive
- `N` — button is active

Default — `N` ||
|| **CONTEXT**
[`string`](../../data-types.md) | Display context.

Allowed values:
- `MOBILE` — show only on mobile devices
- `DESKTOP` — show only in desktop version
- `ALL` — show everywhere
 
Default — `ALL` ||
|| **DISPLAY**
[`string`](../../data-types.md) | Button display.

Allowed values:
 - `LINE` — button in line
 - `BLOCK` — button as a separate block
 
Default — `BLOCK` ||
|| **WIDTH**
[`integer`](../../data-types.md) | Button width in pixels ||
|| **BG_COLOR**
[`string`](../../data-types.md) | Button color in HEX code format ||
|| **BG_COLOR_TOKEN**
[`string`](../../data-types.md) | Button color token.

Allowed values:
 - `primary` — main accent style
 - `secondary` — secondary style
 - `alert` — alert style
 - `base` — basic neutral style
 
Default — `base` ||
|| **TEXT_COLOR**
[`string`](../../data-types.md) | Button text color in HEX code format  ||
|| **OFF_BG_COLOR**
[`string`](../../data-types.md) | Button color in HEX code format in inactive state ||
|| **OFF_TEXT_COLOR**
[`string`](../../data-types.md) | Button text color in HEX code format in inactive state ||
|#

## Example of Sending a Message with a Keyboard from a Chat Bot

{% include [Example Notes](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"BOT_ID":1291,"DIALOG_ID":"chat2725","MESSAGE":"Select an action","URL_PREVIEW":"Y","KEYBOARD":{"BUTTONS":[{"TEXT":"Open Website","LINK":"https://www.example.com/","DISPLAY":"LINE","BG_COLOR_TOKEN":"primary"},{"TEXT":"Task Command","COMMAND":"task","COMMAND_PARAMS":"task #1","DISPLAY":"LINE","BG_COLOR_TOKEN":"secondary"},{"TYPE":"NEWLINE"},{"TEXT":"Insert Text","ACTION":"PUT","ACTION_VALUE":"/task task #1","DISPLAY":"BLOCK","BG_COLOR_TOKEN":"alert","TEXT_COLOR":"#FFFFFF"},{"TEXT":"Neutral Button","ACTION":"SEND","ACTION_VALUE":"Done","DISPLAY":"BLOCK","BG_COLOR_TOKEN":"base"}]},"CLIENT_ID":"**put_your_client_id_here**"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.message.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"BOT_ID":1291,"DIALOG_ID":"chat2725","MESSAGE":"Select an action","URL_PREVIEW":"Y","KEYBOARD":{"BUTTONS":[{"TEXT":"Open Website","LINK":"https://www.example.com/","DISPLAY":"LINE","BG_COLOR_TOKEN":"primary"},{"TEXT":"Task Command","COMMAND":"task","COMMAND_PARAMS":"task #1","DISPLAY":"LINE","BG_COLOR_TOKEN":"secondary"},{"TYPE":"NEWLINE"},{"TEXT":"Insert Text","ACTION":"PUT","ACTION_VALUE":"/task task #1","DISPLAY":"BLOCK","BG_COLOR_TOKEN":"alert","TEXT_COLOR":"#FFFFFF"},{"TEXT":"Neutral Button","ACTION":"SEND","ACTION_VALUE":"Done","DISPLAY":"BLOCK","BG_COLOR_TOKEN":"base"}]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/imbot.message.add
    ```

- JS

    ```js
    try {
        const response = await $b24.callMethod(
            'imbot.message.add',
            {
                BOT_ID: 1291,
                DIALOG_ID: 'chat2725',
                MESSAGE: 'Select an action',
                URL_PREVIEW: 'Y',
                KEYBOARD: {
                    BUTTONS: [
                        {
                            TEXT: 'Open Website',
                            LINK: 'https://www.example.com/',
                            DISPLAY: 'LINE',
                            BG_COLOR_TOKEN: 'primary'
                        },
                        {
                            TEXT: 'Task Command',
                            COMMAND: 'task',
                            COMMAND_PARAMS: 'task #1',
                            DISPLAY: 'LINE',
                            BG_COLOR_TOKEN: 'secondary'
                        },
                        { TYPE: 'NEWLINE' },
                        {
                            TEXT: 'Insert Text',
                            ACTION: 'PUT',
                            ACTION_VALUE: '/task task #1',
                            DISPLAY: 'BLOCK',
                            BG_COLOR_TOKEN: 'alert',
                            TEXT_COLOR: '#FFFFFF'
                        },
                        {
                            TEXT: 'Neutral Button',
                            ACTION: 'SEND',
                            ACTION_VALUE: 'Done',
                            DISPLAY: 'BLOCK',
                            BG_COLOR_TOKEN: 'base'
                        }
                    ]
                }
            }
        );

        const result = response.getData().result;
        console.log('Created message with ID:', result);
        processResult(result);
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
                'imbot.message.add',
                [
                    'BOT_ID' => 1291,
                    'DIALOG_ID' => 'chat2725',
                    'MESSAGE' => 'Select an action',
                    'URL_PREVIEW' => 'Y',
                    'KEYBOARD' => [
                        'BUTTONS' => [
                            [
                                'TEXT' => 'Open Website',
                                'LINK' => 'https://www.example.com/',
                                'DISPLAY' => 'LINE',
                                'BG_COLOR_TOKEN' => 'primary'
                            ],
                            [
                                'TEXT' => 'Task Command',
                                'COMMAND' => 'task',
                                'COMMAND_PARAMS' => 'task #1',
                                'DISPLAY' => 'LINE',
                                'BG_COLOR_TOKEN' => 'secondary'
                            ],
                            ['TYPE' => 'NEWLINE'],
                            [
                                'TEXT' => 'Insert Text',
                                'ACTION' => 'PUT',
                                'ACTION_VALUE' => '/task task #1',
                                'DISPLAY' => 'BLOCK',
                                'BG_COLOR_TOKEN' => 'alert',
                                'TEXT_COLOR' => '#FFFFFF'
                            ],
                            [
                                'TEXT' => 'Neutral Button',
                                'ACTION' => 'SEND',
                                'ACTION_VALUE' => 'Done',
                                'DISPLAY' => 'BLOCK',
                                'BG_COLOR_TOKEN' => 'base'
                            ]
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
        echo 'Error adding message: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.message.add',
        {
            BOT_ID: 1291,
            DIALOG_ID: 'chat2725',
            MESSAGE: 'Select an action',
            URL_PREVIEW: 'Y',
            KEYBOARD: {
                BUTTONS: [
                    {
                        TEXT: 'Open Website',
                        LINK: 'https://www.example.com/',
                        DISPLAY: 'LINE',
                        BG_COLOR_TOKEN: 'primary'
                    },
                    {
                        TEXT: 'Task Command',
                        COMMAND: 'task',
                        COMMAND_PARAMS: 'task #1',
                        DISPLAY: 'LINE',
                        BG_COLOR_TOKEN: 'secondary'
                    },

                    { TYPE: 'NEWLINE' },

                    {
                        TEXT: 'Insert Text',
                        ACTION: 'PUT',
                        ACTION_VALUE: '/task task #1',
                        DISPLAY: 'BLOCK',
                        BG_COLOR_TOKEN: 'alert',
                        TEXT_COLOR: '#FFFFFF'
                    },
                    {
                        TEXT: 'Neutral Button',
                        ACTION: 'SEND',
                        ACTION_VALUE: 'Done',
                        DISPLAY: 'BLOCK',
                        BG_COLOR_TOKEN: 'base'
                    }
                ]
            }
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
        'imbot.message.add',
        [
            'BOT_ID' => 1291,
            'DIALOG_ID' => 'chat2725',
            'MESSAGE' => 'Select an action',
            'URL_PREVIEW' => 'Y',
            'KEYBOARD' => [
                'BUTTONS' => [
                    [
                        'TEXT' => 'Open Website',
                        'LINK' => 'https://www.example.com/',
                        'DISPLAY' => 'LINE',
                        'BG_COLOR_TOKEN' => 'primary'
                    ],
                    [
                        'TEXT' => 'Task Command',
                        'COMMAND' => 'task',
                        'COMMAND_PARAMS' => 'task #1',
                        'DISPLAY' => 'LINE',
                        'BG_COLOR_TOKEN' => 'secondary'
                    ],
                    ['TYPE' => 'NEWLINE'],
                    [
                        'TEXT' => 'Insert Text',
                        'ACTION' => 'PUT',
                        'ACTION_VALUE' => '/task task #1',
                        'DISPLAY' => 'BLOCK',
                        'BG_COLOR_TOKEN' => 'alert',
                        'TEXT_COLOR' => '#FFFFFF'
                    ],
                    [
                        'TEXT' => 'Neutral Button',
                        'ACTION' => 'SEND',
                        'ACTION_VALUE' => 'Done',
                        'DISPLAY' => 'BLOCK',
                        'BG_COLOR_TOKEN' => 'base'
                    ]
                ]
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## How to Update or Remove a Keyboard

To update the keyboard buttons, use the following methods:

- [im.message.update](./im-message-update.md)
- [imbot.message.update](../../chat-bots/messages/imbot-message-update.md)

To disable the display of buttons, pass:

- `KEYBOARD: 'N'`
- an empty value for `KEYBOARD`

## Command Processing by the Chat Bot {#command-processing}

1. To ensure the command works with the keyboard, register it using the [imbot.command.register](../../chat-bots/commands/imbot-command-register.md) method.

    In the button, specify the following keys:

    ```php
    "COMMAND" => "example", // command that will be sent to the chat bot
    "COMMAND_PARAMS" => "example", // parameters for the command
     ```  

2. Pressing the button will generate the [ONIMCOMMANDADD](../../chat-bots/commands/events/on-im-command-add.md) event.
3. Inside the event, the array `data[COMMAND]` will contain data about the invoked event. The value `COMMAND_CONTEXT` will indicate the context in which the command was invoked:
   - `TEXTAREA` — command entered manually
   - `KEYBOARD` — command invoked by button
   - `MENU` — command invoked from the context menu