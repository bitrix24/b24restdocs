# Working with Keyboards

The keyboard consists of buttons beneath the message. They can be used to open links, perform actions, and trigger commands.

Methods that support keyboard functionality:

**Chatbots 2.0 (`imbot.v2`)**

- [imbot.v2.Chat.Message.send](./chat-message-send.md) — send a message on behalf of the chatbot
- [imbot.v2.Chat.Message.update](./chat-message-update.md) — modify a sent message from the chatbot
- [imbot.v2.Command.answer](../commands/command-answer.md) — send a response to a chatbot command

**Chats (`im`)**

- [im.message.add](../../../../chats/messages/im-message-add.md) — send a message in a chat
- [im.message.update](../../../../chats/messages/im-message-update.md) — modify a sent message

**Deprecated Chatbots (`imbot`)**

- [imbot.message.add](../../../outdated/messages/imbot-message-add.md) — send a message on behalf of the chatbot
- [imbot.message.update](../../../outdated/messages/imbot-message-update.md) — modify a sent message from the chatbot
- [imbot.command.answer](../../../outdated/commands/imbot-command-answer.md) — send a response to a chatbot command

## How to Add a Keyboard

To add a keyboard, pass the `KEYBOARD` parameter when creating or updating a message.

`KEYBOARD` can be passed as:

- a JSON string
- an object with the root key `BUTTONS`
- an array of buttons without wrapping

If the `KEYBOARD` does not contain the `BUTTONS` key, the server will automatically assume that a shortened format has been provided and will wrap the array in `BUTTONS`.

{% list tabs %}

- Full format with the `BUTTONS` key

  ```json
  {
      "KEYBOARD": {
          "BUTTONS": [
              {"TEXT": "Button", "LINK": "https://example.com"}
          ]
      }
  }
  ```

- Shortened format

  ```json
  {
      "KEYBOARD": [
          {"TEXT": "Button", "LINK": "https://example.com"}
      ]
  }
  ```

{% endlist %}

## Button Fields

#|
|| **Name**
`type` | **Description** ||
|| **TEXT**
[`string`](../../../../data-types.md) | Button text.

For all buttons except `TYPE`, it is mandatory to specify `TEXT` and one action field: `LINK`, `COMMAND`, `ACTION + ACTION_VALUE`, or `APP_ID` ||
|| **TYPE**
[`string`](../../../../data-types.md) | Move the button to a new line. The only allowed value is `NEWLINE` ||
|| **LINK**
[`string`](../../../../data-types.md) | Button link. `http/https` and relative path `/...` are allowed ||
|| **APP_ID**
[`integer`](../../../../data-types.md) | Application identifier for the chat.

Deprecated scenario. To open an application from the chat, use widgets ||
|| **APP_PARAMS**
[`string`](../../../../data-types.md) | Parameters for launching the application in the chat. Pass together with `APP_ID`.

Deprecated scenario. To open an application from the chat, use widgets ||
|| **ACTION**
[`string`](../../../../data-types.md) | Action:

- `PUT` — insert text into the input field
- `SEND` — send text
- `COPY` — copy text to the clipboard
- `CALL` — make a call
- `DIALOG` — open a chat ||
|| **ACTION_VALUE**
[`string`](../../../../data-types.md) | Value for `ACTION`:

- `PUT` — text to insert into the input field
- `SEND` — text to send
- `COPY` — text to copy
- `CALL` — phone number in international format
- `DIALOG` — chat identifier (`chatXXX`) or `ID` of the user for a personal chat ||
|| **COMMAND**
[`string`](../../../../data-types.md) | Command for the bot. More about processing — [below](#command-processing) ||
|| **COMMAND_PARAMS**
[`string`](../../../../data-types.md) | Command parameters. Pass together with `COMMAND` ||
|| **BLOCK**
[`string`](../../../../data-types.md) | Button blocking:

- `Y` — block after pressing
- `N` — do not block

Default is `N` ||
|| **DISABLED**
[`string`](../../../../data-types.md) | Button activity:

- `Y` — button is inactive
- `N` — button is active

Default is `N` ||
|| **CONTEXT**
[`string`](../../../../data-types.md) | Display context:

- `MOBILE` — mobile device only
- `DESKTOP` — desktop only
- `ALL` — everywhere

Default is `ALL` ||
|| **DISPLAY**
[`string`](../../../../data-types.md) | Button display:

- `LINE` — button in line
- `BLOCK` — button as a separate block

Default is `BLOCK` ||
|| **WIDTH**
[`integer`](../../../../data-types.md) | Button width in pixels ||
|| **BG_COLOR**
[`string`](../../../../data-types.md) | Button color in HEX format ||
|| **BG_COLOR_TOKEN**
[`string`](../../../../data-types.md) | Button color token:

- `primary`
- `secondary`
- `alert`
- `base`

Default is `base` ||
|| **TEXT_COLOR**
[`string`](../../../../data-types.md) | Button text color in HEX format ||
|| **OFF_BG_COLOR**
[`string`](../../../../data-types.md) | Button color in inactive state ||
|| **OFF_TEXT_COLOR**
[`string`](../../../../data-types.md) | Button text color in inactive state ||
|#

{% note info "" %}

The option with `APP_ID` and `APP_PARAMS` parameters is used in chats of [Open Channels](../../../../imopenlines/openlines/index.md).

{% endnote %}

## Example of Sending a Message with a Keyboard

{% include [Example Note](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"botId":456,"botToken":"my_bot_token","dialogId":"chat2725","fields":{"message":"Select an action","keyboard":{"BUTTONS":[{"TEXT":"Open website","LINK":"https://www.example.com/"},{"TYPE":"NEWLINE"},{"TEXT":"Insert command","ACTION":"PUT","ACTION_VALUE":"/help"}]}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.v2.Chat.Message.send
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"botId":456,"dialogId":"chat2725","fields":{"message":"Select an action","keyboard":{"BUTTONS":[{"TEXT":"Open website","LINK":"https://www.example.com/"},{"TYPE":"NEWLINE"},{"TEXT":"Insert command","ACTION":"PUT","ACTION_VALUE":"/help"}]}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/imbot.v2.Chat.Message.send
  ```

- JS

  ```js
  try {
      const response = await $b24.callMethod('imbot.v2.Chat.Message.send', {
          botId: 456,
          dialogId: 'chat2725',
          fields: {
              message: 'Select an action',
              keyboard: {
                  BUTTONS: [
                      { TEXT: 'Open website', LINK: 'https://www.example.com/' },
                      { TYPE: 'NEWLINE' },
                      { TEXT: 'Insert command', ACTION: 'PUT', ACTION_VALUE: '/help' },
                  ],
              },
          },
      });

      const result = response.getData().result.id;
      console.log('Created message ID:', result);
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
              'imbot.v2.Chat.Message.send',
              [
                  'botId' => 456,
                  'dialogId' => 'chat2725',
                  'fields' => [
                      'message' => 'Select an action',
                      'keyboard' => [
                          'BUTTONS' => [
                              ['TEXT' => 'Open website', 'LINK' => 'https://www.example.com/'],
                              ['TYPE' => 'NEWLINE'],
                              ['TEXT' => 'Insert command', 'ACTION' => 'PUT', 'ACTION_VALUE' => '/help'],
                          ],
                      ],
                  ],
              ]
          );

      $result = $response
          ->getResponseData()
          ->getResult()['id'];

      echo 'Created message ID: ' . $result;
  } catch (Throwable $e) {
      error_log($e->getMessage());
      echo 'Error: ' . $e->getMessage();
  }
  ```

- BX24.js

  ```js
  BX24.callMethod(
      'imbot.v2.Chat.Message.send',
      {
          botId: 456,
          dialogId: 'chat2725',
          fields: {
              message: 'Select an action',
              keyboard: {
                  BUTTONS: [
                      { TEXT: 'Open website', LINK: 'https://www.example.com/' },
                      { TYPE: 'NEWLINE' },
                      { TEXT: 'Insert command', ACTION: 'PUT', ACTION_VALUE: '/help' },
                  ],
              },
          },
      },
      function(result) {
          if (result.error()) {
              console.error(result.error().ex);
          } else {
              console.log('Message ID:', result.data().id);
          }
      }
  );
  ```

- PHP CRest

  ```php
  require_once('crest.php');

  $result = CRest::call(
      'imbot.v2.Chat.Message.send',
      [
          'botId' => 456,
          'dialogId' => 'chat2725',
          'fields' => [
              'message' => 'Select an action',
              'keyboard' => [
                  'BUTTONS' => [
                      ['TEXT' => 'Open website', 'LINK' => 'https://www.example.com/'],
                      ['TYPE' => 'NEWLINE'],
                      ['TEXT' => 'Insert command', 'ACTION' => 'PUT', 'ACTION_VALUE' => '/help'],
                  ],
              ],
          ],
      ]
  );

  if (!empty($result['error'])) {
      echo 'Error: ' . $result['error_description'];
  } else {
      echo 'Message ID: ' . $result['result']['id'];
  }
  ```

{% endlist %}

## How to Update or Remove a Keyboard

To update buttons, use the following methods:

- [imbot.v2.Chat.Message.update](./chat-message-update.md)
- [im.message.update](../../../../chats/messages/im-message-update.md)
- [imbot.message.update](../../../outdated/messages/imbot-message-update.md)

To disable button display, pass:

- `KEYBOARD: 'N'`
- an empty value for `KEYBOARD`

## Command Processing by the Chatbot {#command-processing}

1. To ensure the command works with the keyboard, register it using the method [imbot.v2.Command.register](../commands/command-register.md).

2. Pressing a button will generate the event [ONIMBOTV2COMMANDADD](../events/events.md#onimbotv2commandadd).

3. In the event, the value of `command.context` indicates the context in which the command was invoked:
- `textarea` — entered manually
- `keyboard` — invoked by a button
- `menu` — invoked from the context menu