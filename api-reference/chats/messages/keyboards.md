# Working with Keyboards

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

`KEYBOARD` adds interactive buttons to a message: opening a link, inserting text into the input field, making a call, and other actions.

The keyboard is passed in the `KEYBOARD` parameter when sending or updating a message: [im.message.add](./im-message-add.md), [im.message.update](./im-message-update.md).

## What You Can Do

- open a link: `LINK`
- insert or send text, copy it, make a call, open a chat: `ACTION`
- run a chatbot command: `COMMAND` — works only in the keyboard of the bot itself
- move the following buttons to a new line: `TYPE`

Buttons are needed when the user is expected to act in response to the message. For other tasks, the section has its own mechanisms:

- format the message text — [formatting](./formatting.md)
- attach structured blocks, images, or tables — [attachments](./attachments.md)
- add items to the message context menu — [menu](./menu.md)

## Button Fields {#button-fields}

Buttons are listed in the `KEYBOARD.BUTTONS` array. Bitrix24 also accepts shorthand forms: an array of buttons without the `BUTTONS` wrapper, and the same structure as a JSON string — the wrapper is added automatically.

A regular button requires `TEXT` and at least one action: `LINK`, `APP_ID`, the `ACTION` and `ACTION_VALUE` pair, or `COMMAND`.

#|
|| **Name**
`type` | **Description** ||
|| **TEXT**
[`string`](../../data-types.md) | Button text ||
|| **LINK**
[`string`](../../data-types.md) | Link. Only addresses starting with `http://`, `https://`, or `/` are accepted ||
|| **ACTION**
[`string`](../../data-types.md) | Button action:

- `PUT` — insert text into the input field
- `SEND` — send text
- `COPY` — copy text to the clipboard
- `CALL` — make a call
- `DIALOG` — open a chat ||
|| **ACTION_VALUE**
[`string`](../../data-types.md) | Value for `ACTION`: text for `PUT`, `SEND`, and `COPY`, a phone number for `CALL`, a dialog ID for `DIALOG`. Passed only together with `ACTION` and cannot be empty ||
|| **COMMAND**
[`string`](../../data-types.md) | Chatbot command. The leading `/` can be omitted: Bitrix24 removes it.

The [im.message.add](./im-message-add.md) and [im.message.update](./im-message-update.md) methods send a message on behalf of the user, so a button with `COMMAND` does not make it into the message — see the details [below](#dropped-buttons) ||
|| **COMMAND_PARAMS**
[`string`](../../data-types.md) | Command parameters. Passed together with `COMMAND` ||
|| **APP_ID**
[`integer`](../../data-types.md) | ID of the chat application.

Legacy scenario: the server accepts such a button, but the web messenger does not open the application from it. To open the application interface from a chat, use [messenger widgets](../../widgets/im/index.md) ||
|| **APP_PARAMS**
[`string`](../../data-types.md) | Application launch parameters. Passed together with `APP_ID` ||
|| **TYPE**
[`string`](../../data-types.md) | Turns the array element into a service separator button. The only value is `NEWLINE`, described [below](#newline) ||
|#

Besides the listed ones, `ACTION` accepts the service values `LIVECHAT` and `HELP`. The documentation does not describe their behavior in the interface — use the values from the table in your keyboards.

Ten more fields define the appearance and state of a button: `BLOCK`, `DISABLED`, `CONTEXT`, `DISPLAY`, `WIDTH`, `BG_COLOR`, `BG_COLOR_TOKEN`, `TEXT_COLOR`, `OFF_BG_COLOR`, `OFF_TEXT_COLOR`. They do not affect the button action; they are described in the [Keyboards in Messages](../../chat-bots/chat-bots-v2/imbot.v2/messages/message-keyboards.md) article.

The serialized keyboard must be shorter than 60,000 characters. A larger keyboard does not make it into the message, and [im.message.update](./im-message-update.md) returns the `KEYBOARD_OVERSIZE` error.

### Which Buttons Do Not Make It into the Message {#dropped-buttons}

A button does not make it into the message in two cases: it has no text, or Bitrix24 did not recognize any action. Unknown fields do not interfere: they have no effect on the button.

The button text cannot be empty, consist only of spaces, or be the string `"0"`. Such a button does not make it into the message, whatever action you set.

The action is determined by the first suitable field in the order `LINK`, `APP_ID`, `ACTION`, `COMMAND`. An invalid field does not cause the button to be dropped — the check proceeds to the next field. For example, a button with an invalid link and a correct `ACTION` and `ACTION_VALUE` pair is sent: the action works, and the link is skipped.

Bitrix24 does not recognize the action if:

- the `ACTION` value is not among the allowed ones, or `ACTION` is passed without `ACTION_VALUE`
- `LINK` failed the format check
- only `COMMAND` is passed, and the message is sent by the `im.message.*` methods. Send buttons with commands using the methods of the [Chatbots 2.0](../../chat-bots/chat-bots-v2/imbot.v2/messages/index.md) section

{% note warning "" %}

There is no error in this case. If at least one working button remains in the keyboard, the method returns `200` and sends the message without the lost buttons. The `KEYBOARD_ERROR` error is returned only when no buttons remain

{% endnote %}

To verify that the entire keyboard was created, retrieve the sent message with the [im.dialog.messages.get](./im-dialog-messages-get.md) method: the keyboard is returned in `result.messages[].params.KEYBOARD`.

You cannot compare the response with the request verbatim: Bitrix24 adds service fields to every button, including `TYPE`, `BOT_ID`, `BLOCK`, `DISABLED`, `DISPLAY`, `CONTEXT`. Compare the set of buttons and their actions.

### Line Break {#newline}

The `{"TYPE": "NEWLINE"}` button is not displayed and performs no actions — it moves the following buttons to a new line. It has no other fields: `TEXT` and an action are not required.

When a message is retrieved, the separator is returned unchanged, and regular buttons get the service field `TYPE` with the value `BUTTON`.

## Example of Sending a Message with a Keyboard

{% include [Footnote on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DIALOG_ID":"chat2725","MESSAGE":"Select an action","KEYBOARD":{"BUTTONS":[{"TEXT":"Open website","LINK":"https://www.example.com/"},{"TYPE":"NEWLINE"},{"TEXT":"Insert command","ACTION":"PUT","ACTION_VALUE":"/help"}]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.message.add
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DIALOG_ID":"chat2725","MESSAGE":"Select an action","KEYBOARD":{"BUTTONS":[{"TEXT":"Open website","LINK":"https://www.example.com/"},{"TYPE":"NEWLINE"},{"TEXT":"Insert command","ACTION":"PUT","ACTION_VALUE":"/help"}]},"auth":"**put_access_token_here**"}' \
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
          MESSAGE: 'Choose an action',
          KEYBOARD: {
            BUTTONS: [
              { TEXT: 'Open site', LINK: 'https://www.example.com/' },
              { TYPE: 'NEWLINE' },
              { TEXT: 'Insert command', ACTION: 'PUT', ACTION_VALUE: '/help' },
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
        console.info('Created message ID:', result)
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
      async function sendMessageWithKeyboard() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'im.message.add',
            params: {
              DIALOG_ID: 'chat2725',
              MESSAGE: 'Choose an action',
              KEYBOARD: {
                BUTTONS: [
                  { TEXT: 'Open site', LINK: 'https://www.example.com/' },
                  { TYPE: 'NEWLINE' },
                  { TEXT: 'Insert command', ACTION: 'PUT', ACTION_VALUE: '/help' },
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
          console.info('Created message ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', sendMessageWithKeyboard)
    </script>
    ```

- Python

  ```python
  from b24pysdk.errors import BitrixAPIError, BitrixSDKException

  try:
      bitrix_response = client.im.message.add(
          dialog_id="chat2725",
          message="Select an action",
          keyboard={
              "BUTTONS": [
                  {
                      "TEXT": "Open the website",
                      "LINK": "https://www.example.com/",
                  },
                  {
                      "TYPE": "NEWLINE",
                  },
                  {
                      "TEXT": "Insert the command",
                      "ACTION": "PUT",
                      "ACTION_VALUE": "/help",
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
                  'MESSAGE' => 'Select an action',
                  'KEYBOARD' => [
                      'BUTTONS' => [
                          ['TEXT' => 'Open website', 'LINK' => 'https://www.example.com/'],
                          ['TYPE' => 'NEWLINE'],
                          ['TEXT' => 'Insert command', 'ACTION' => 'PUT', 'ACTION_VALUE' => '/help'],
                      ],
                  ],
              ]
          );

      $result = $response
          ->getResponseData()
          ->getResult();

      echo 'Created message ID: ' . $result;
  } catch (Throwable $e) {
      error_log($e->getMessage());
      echo 'Error: ' . $e->getMessage();
  }
  ```

- BX24.js

  ```js
  BX24.callMethod(
      'im.message.add',
      {
          DIALOG_ID: 'chat2725',
          MESSAGE: 'Select an action',
          KEYBOARD: {
              BUTTONS: [
                  { TEXT: 'Open website', LINK: 'https://www.example.com/' },
                  { TYPE: 'NEWLINE' },
                  { TEXT: 'Insert command', ACTION: 'PUT', ACTION_VALUE: '/help' },
              ],
          },
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
          'MESSAGE' => 'Select an action',
          'KEYBOARD' => [
              'BUTTONS' => [
                  ['TEXT' => 'Open website', 'LINK' => 'https://www.example.com/'],
                  ['TYPE' => 'NEWLINE'],
                  ['TEXT' => 'Insert command', 'ACTION' => 'PUT', 'ACTION_VALUE' => '/help'],
              ],
          ],
      ]
  );

  print_r($result);
  ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "im.message.add", b24.Params{
    	"DIALOG_ID": "chat2725",
    	"MESSAGE":   "Select an action",
    	"KEYBOARD": b24.Params{
    		"BUTTONS": []b24.Params{
    			{
    				"TEXT": "Open website",
    				"LINK": "https://www.example.com/",
    			},
    			{
    				"TYPE": "NEWLINE",
    			},
    			{
    				"TEXT":         "Insert command",
    				"ACTION":       "PUT",
    				"ACTION_VALUE": "/help",
    			},
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("im.message.add: %w", err)
    }

    // The response comes as json.RawMessage — parse it into a struct
    // matching the response shape on the im.message.add method page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

{% note warning "" %}

The current documentation on keyboards can be found in the Chatbots 2.0 section:

- [Keyboards in Messages](../../chat-bots/chat-bots-v2/imbot.v2/messages/message-keyboards.md)

{% endnote %}

## Continue Learning

- [{#T}](../../chat-bots/chat-bots-v2/imbot.v2/messages/index.md)
- [{#T}](../../chat-bots/chat-bots-v2/imbot.v2/messages/chat-message-send.md)