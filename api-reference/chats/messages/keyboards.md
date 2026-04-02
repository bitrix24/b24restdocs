# Working with Keyboards

`KEYBOARD` adds interactive buttons to a message: link navigation, command execution, text substitution in the input field, and other actions.

The keyboard is passed in the `KEYBOARD` parameter when sending or updating a message: [im.message.add](./im-message-add.md), [im.message.update](./im-message-update.md).

## Minimum Structure

- `KEYBOARD.BUTTONS` — array of buttons
- `TEXT` — button text
- one action: `LINK` or `ACTION` + `ACTION_VALUE` or `COMMAND`

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

- JS

  ```js
  try {
      const response = await $b24.callMethod(
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
          }
      );

      const result = response.getData().result;
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

{% endlist %}

{% note warning "" %}

The current documentation on keyboards can be found in the Chatbots 2.0 section:

- [Keyboards in Messages](../../chat-bots/chat-bots-v2/imbot.v2/messages/message-keyboards.md)

{% endnote %}

## Continue Learning

- [{#T}](../../chat-bots/chat-bots-v2/imbot.v2/messages/index.md)
- [{#T}](../../chat-bots/chat-bots-v2/imbot.v2/messages/chat-message-send.md)