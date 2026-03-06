# Working with the Context Menu

The context menu is a set of actions within a message. You can add your own items to the context menu to open links or send commands to the bot.

Methods that support working with the context menu:

- [imbot.message.add](../../chat-bots/messages/imbot-message-add.md) — send a message on behalf of the chat bot
- [imbot.message.update](../../chat-bots/messages/imbot-message-update.md) — modify a sent message from the chat bot
- [imbot.command.answer](../../chat-bots/commands/imbot-command-answer.md) — send a response to a command from the chat bot
- [im.message.add](./im-message-add.md) — send a message in the chat
- [im.message.update](./im-message-update.md) — modify a sent message

## How to Add an Item to the Context Menu

To add an item to the context menu, pass the `MENU` parameter when creating or updating a message.

`MENU` can be passed as:

- a JSON string
- an object with the root key `ITEMS`
- an array of items without wrapping

If the `MENU` does not contain the `ITEMS` key, the server will automatically assume that a shortened format has been provided and will wrap the array in `ITEMS`.

{% list tabs %}

- Full format with the ITEMS key

  ```json
  {
      "MENU": {
          "ITEMS": [
              { "TEXT": "Open Website", "LINK": "https://example.com" }
          ]
      }
  }
  ```

- Shortened format

  ```json
  {
      "MENU": [
          { "TEXT": "Open Website", "LINK": "https://example.com" }
      ]
  }
  ```

{% endlist %}

## Menu Item Fields

#|
|| **Name**
`type` | **Description** ||
|| **TEXT**
[`string`](../../data-types.md) | The text of the menu item.

For menu items, it is mandatory to specify `TEXT` and one action field — `LINK`, `COMMAND`, `ACTION + ACTION_VALUE`, or `APP_ID` ||
|| **LINK**
[`string`](../../data-types.md) | The link for the menu item. `http/https` and relative paths `/...` are allowed. ||
|| **COMMAND**
[`string`](../../data-types.md) | Command for the bot.

For more details on command processing by the chat bot, read [below](#command-processing) ||
|| **COMMAND_PARAMS**
[`string`](../../data-types.md) | Command parameters. Pass together with `COMMAND` ||
|| **APP_ID**
[`integer`](../../data-types.md) | Application identifier for the chat.

Deprecated scenario. To open an application from the chat, use widgets. ||
|| **APP_PARAMS**
[`string`](../../data-types.md) | Parameters for launching the application in the chat. Pass together with `APP_ID`. 

Deprecated scenario. To open an application from the chat, use widgets.

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
- `DIALOG` — chat identifier in the format `chatXXX` for group chats and `ID` of the user for personal chats

Available starting from [REST API IM revision](../im-revision-get.md) 28 ||
|| **CONTEXT**
[`string`](../../data-types.md) | Display context.

Allowed values:
- `MOBILE` — show only on mobile devices
- `DESKTOP` — show only in the desktop version
- `ALL` — show everywhere

Default is `ALL` ||
|| **DISABLED**
[`string`](../../data-types.md) | Activity of the menu item.

Allowed values:
- `Y` — menu item is inactive
- `N` — menu item is active

Default is `N` ||
|#

## Example of Sending a Message with a Context Menu

{% include [Example Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DIALOG_ID":"chat2725","MESSAGE":"Select an action from the menu","URL_PREVIEW":"Y","MENU":{"ITEMS":[{"TEXT":"Open Website","LINK":"https://www.example.com/"},{"TEXT":"Send Text","ACTION":"SEND","ACTION_VALUE":"Done"}]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.message.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DIALOG_ID":"chat2725","MESSAGE":"Select an action from the menu","URL_PREVIEW":"Y","MENU":{"ITEMS":[{"TEXT":"Open Website","LINK":"https://www.example.com/"},{"TEXT":"Send Text","ACTION":"SEND","ACTION_VALUE":"Done"}]}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/im.message.add
    ```

- JS

    ```js
    try {
        const response = await $b24.callMethod(
            'im.message.add',
            {
                DIALOG_ID: 'chat2725',
                MESSAGE: 'Select an action from the menu',
                URL_PREVIEW: 'Y',
                MENU: {
                    ITEMS: [
                        {
                            TEXT: 'Open Website',
                            LINK: 'https://www.example.com/'
                        },
                        {
                            TEXT: 'Send Text',
                            ACTION: 'SEND',
                            ACTION_VALUE: 'Done'
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
                'im.message.add',
                [
                    'DIALOG_ID' => 'chat2725',
                    'MESSAGE' => 'Select an action from the menu',
                    'URL_PREVIEW' => 'Y',
                    'MENU' => [
                        'ITEMS' => [
                            [
                                'TEXT' => 'Open Website',
                                'LINK' => 'https://www.example.com/'
                            ],
                            [
                                'TEXT' => 'Send Text',
                                'ACTION' => 'SEND',
                                'ACTION_VALUE' => 'Done'
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
        'im.message.add',
        {
            DIALOG_ID: 'chat2725',
            MESSAGE: 'Select an action from the menu',
            URL_PREVIEW: 'Y',
            MENU: {
                ITEMS: [
                    {
                        TEXT: 'Open Website',
                        LINK: 'https://www.example.com/'
                    },
                    {
                        TEXT: 'Send Text',
                        ACTION: 'SEND',
                        ACTION_VALUE: 'Done',
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
        'im.message.add',
        [
            'DIALOG_ID' => 'chat2725',
            'MESSAGE' => 'Select an action from the menu',
            'URL_PREVIEW' => 'Y',
            'MENU' => [
                'ITEMS' => [
                    [
                        'TEXT' => 'Open Website',
                        'LINK' => 'https://www.example.com/'
                    ],
                    [
                        'TEXT' => 'Send Text',
                        'ACTION' => 'SEND',
                        'ACTION_VALUE' => 'Done'
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

## How to Update or Remove the Context Menu

To update menu items, use the following methods:

- [im.message.update](./im-message-update.md)
- [imbot.message.update](../../chat-bots/messages/imbot-message-update.md)

To disable the display of additional menu items, pass:

- `MENU: 'N'`
- an empty value for `MENU`

## Command Processing by the Chat Bot {#command-processing}

1. To ensure the command works in the menu, register it using the method [imbot.command.register](../../chat-bots/commands/imbot-command-register.md).

    In the menu item, specify the following keys:

    ```php
    "COMMAND" => "example", // command that will be sent to the chat bot
    "COMMAND_PARAMS" => "example", // parameters for the command
    ```  
     
2. Clicking on the menu item will generate the event [ONIMCOMMANDADD](../../chat-bots/commands/events/on-im-command-add.md).
3. Inside the event, the array `data[COMMAND]` will contain data about the invoked event. The value `COMMAND_CONTEXT` will indicate the context in which the command was invoked:
   - `TEXTAREA` — command entered manually
   - `KEYBOARD` — command invoked by button
   - `MENU` — command invoked from the context menu