# Working with Context Menu

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The context menu is a list of actions for a message. Custom menu items can open a link, insert or copy text, make a call, open a chat, or send a command to a bot.

The menu is passed in the `MENU` parameter when a message is sent or updated:

- [im.message.add](./im-message-add.md) — send a message in chat
- [im.message.update](./im-message-update.md) — modify a sent message

[Keyboard](./keyboards.md) buttons are visible under the message right away, while menu items appear only when the user opens the message menu. Use a keyboard for an action expected in response to the message, and a menu for additional actions that should not take up space in the chat.

A menu item with the `COMMAND` bot command works only in messages sent by the chat bot itself. Such messages are sent by the deprecated methods [imbot.message.add](../../chat-bots/outdated/messages/imbot-message-add.md), [imbot.message.update](../../chat-bots/outdated/messages/imbot-message-update.md), and [imbot.command.answer](../../chat-bots/outdated/commands/imbot-command-answer.md). The [Chatbots 2.0](../../chat-bots/chat-bots-v2/imbot.v2/messages/index.md) methods do not support the context menu.

## How to Add an Item to the Context Menu

`MENU` can be passed as:

- a JSON string
- an object with the root key `ITEMS`
- an array of items without wrapping

An array without the `ITEMS` key is the shortened format: the server wraps it in `ITEMS` itself.

{% list tabs %}

- Full format with the key ITEMS

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

Each item requires `TEXT` and one action field: `LINK`, `COMMAND`, `ACTION + ACTION_VALUE`, or `APP_ID` ||
|| **LINK**
[`string`](../../data-types.md) | The link for the menu item. `http/https` and relative paths `/...` are allowed. ||
|| **COMMAND**
[`string`](../../data-types.md) | The command for the chat bot. Works only in messages sent by the chat bot.

How the chat bot processes the command is described in the [Command Processing by the Chat Bot](#command-processing) section ||
|| **COMMAND_PARAMS**
[`string`](../../data-types.md) | Command parameters. Pass together with `COMMAND` ||
|| **APP_ID**
[`integer`](../../data-types.md) | The application identifier for the chat.

Deprecated scenario. To open an application from chat, use [messenger widgets](../../widgets/im/index.md) ||
|| **APP_PARAMS**
[`string`](../../data-types.md) | Parameters for launching the application in chat. Pass together with `APP_ID`.

{% note info "" %}

The `APP_ID` and `APP_PARAMS` parameters are currently used in [Open Channels](../../imopenlines/openlines/index.md) chats — conversations with clients from messengers and social networks

{% endnote %}
||
|| **ACTION**
[`string`](../../data-types.md) | Action:

- `PUT` — insert text into the input field
- `SEND` — send text
- `COPY` — copy text to clipboard
- `CALL` — make a call
- `DIALOG` — open chat

Available starting from [REST API IM revision](../im-revision-get.md) 28 ||
|| **ACTION_VALUE**
[`string`](../../data-types.md) | Value for `ACTION`:

- `PUT` — text to insert into the input field
- `SEND` — text to send
- `COPY` — text to copy to the clipboard
- `CALL` — phone number in international format
- `DIALOG` — chat identifier in the `chatXXX` format for a group chat, user identifier for a personal dialog

Available starting from [REST API IM revision](../im-revision-get.md) 28 ||
|| **CONTEXT**
[`string`](../../data-types.md) | Where to show the menu item.

Allowed values:
- `MOBILE` — show only on mobile devices
- `DESKTOP` — show only in desktop version
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

{% include [Example Notes](../../../_includes/examples.md) %}

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
    -d '{"DIALOG_ID":"chat2725","MESSAGE":"Select an action from the menu","URL_PREVIEW":"Y","MENU":{"ITEMS":[{"TEXT":"Open Website","LINK":"https://www.example.com/"},{"TEXT":"Send Text","ACTION":"SEND","ACTION_VALUE":"Done"}]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/im.message.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<number>({
        method: 'im.message.add',
        params: {
          DIALOG_ID: 'chat2725',
          MESSAGE: 'Select an action from the menu',
          URL_PREVIEW: 'Y',
          MENU: {
            ITEMS: [
              {
                TEXT: 'Open website',
                LINK: 'https://www.example.com/',
              },
              {
                TEXT: 'Send text',
                ACTION: 'SEND',
                ACTION_VALUE: 'Done',
              },
            ],
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Created message with ID:', result)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function addMessageWithMenu() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'im.message.add',
            params: {
              DIALOG_ID: 'chat2725',
              MESSAGE: 'Select an action from the menu',
              URL_PREVIEW: 'Y',
              MENU: {
                ITEMS: [
                  {
                    TEXT: 'Open website',
                    LINK: 'https://www.example.com/',
                  },
                  {
                    TEXT: 'Send text',
                    ACTION: 'SEND',
                    ACTION_VALUE: 'Done',
                  },
                ],
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Created message with ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addMessageWithMenu)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.im.message.add(
            dialog_id="chat2725",
            message="Select an action from the menu",
            url_preview=True,
            menu={
                "ITEMS": [
                    {
                        "TEXT": "Open the website",
                        "LINK": "https://www.example.com/",
                    },
                    {
                        "TEXT": "Send the text",
                        "ACTION": "SEND",
                        "ACTION_VALUE": "Done",
                    },
                ],
            },
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "im.message.add", b24.Params{
    	"DIALOG_ID":   "chat2725",
    	"MESSAGE":     "Select an action from the menu",
    	"URL_PREVIEW": "Y",
    	"MENU": b24.Params{
    		"ITEMS": []b24.Params{
    			{
    				"TEXT": "Open Website",
    				"LINK": "https://www.example.com/",
    			},
    			{
    				"TEXT":         "Send Text",
    				"ACTION":       "SEND",
    				"ACTION_VALUE": "Done",
    			},
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("im.message.add: %w", err)
    }

    // The response arrives as json.RawMessage: result contains the identifier
    // of the created message
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## How to Update or Remove the Context Menu

You can change menu items only in your own message and only within three days after sending it — the same as the message text. Use these methods:

- [im.message.update](./im-message-update.md)
- [imbot.message.update](../../chat-bots/outdated/messages/imbot-message-update.md)

To remove custom items from the menu, pass `MENU: 'N'` or an empty string `MENU: ''`.

## Command Processing by the Chat Bot {#command-processing}

The [im.message.add](./im-message-add.md) and [im.message.update](./im-message-update.md) methods do not retain an item whose only action is `COMMAND`. If `MENU` contains no other items, the method returns an error: `MENU_ERROR` for `im.message.add`, `menu_ERROR` for `im.message.update`.

1. To make the command work from the menu, register it with the [imbot.command.register](../../chat-bots/outdated/commands/imbot-command-register.md) method

    In the menu item, specify the keys:

    ```php
    "COMMAND" => "example", // command that will be sent to the chat bot
    "COMMAND_PARAMS" => "example", // parameters for the command
    ```

2. When the user clicks the menu item, the [ONIMCOMMANDADD](../../chat-bots/outdated/commands/events/on-im-command-add.md) event arrives
3. The event data arrives in the `data[COMMAND][<COMMAND_ID>]` array. The `COMMAND_CONTEXT` value shows where the command was invoked from:
   - `TEXTAREA` — command entered manually
   - `KEYBOARD` — command invoked by button
   - `MENU` — command invoked from the context menu

## Continue Learning

- [{#T}](./im-message-add.md)
- [{#T}](./im-message-update.md)
- [{#T}](./keyboards.md)
- [{#T}](./attachments.md)
- [{#T}](./formatting.md)
- [{#T}](./index.md)