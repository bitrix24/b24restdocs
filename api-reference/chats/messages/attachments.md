# Attachments in Messages

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

`ATTACH` allows you to send structured content in a message: text blocks, links, images, files, dividers, and tables.

Attachments are passed in the `ATTACH` parameter of the [im.message.add](./im-message-add.md) and [im.message.update](./im-message-update.md) methods.

## ATTACH Formats

- Full form: an object with `BLOCKS`
- Short form: an array of blocks without wrapping

## Example of Sending a Message with ATTACH

{% include [Footnote on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DIALOG_ID":"chat2725","MESSAGE":"Card","ATTACH":{"ID":1,"COLOR_TOKEN":"primary","BLOCKS":[{"MESSAGE":"[B]New Request[/B]"},{"LINK":{"NAME":"Open","LINK":"https://example.com"}}]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.message.add
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DIALOG_ID":"chat2725","MESSAGE":"Card","ATTACH":{"ID":1,"COLOR_TOKEN":"primary","BLOCKS":[{"MESSAGE":"[B]New Request[/B]"},{"LINK":{"NAME":"Open","LINK":"https://example.com"}}]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/im.message.add
  ```

- JS

  ```js
  try {
      const response = await $b24.callMethod(
          'im.message.add',
          {
              DIALOG_ID: 'chat2725',
              MESSAGE: 'Card',
              ATTACH: {
                  ID: 1,
                  COLOR_TOKEN: 'primary',
                  BLOCKS: [
                      { MESSAGE: '[B]New Request[/B]' },
                      { LINK: { NAME: 'Open', LINK: 'https://example.com' } },
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
                  'MESSAGE' => 'Card',
                  'ATTACH' => [
                      'ID' => 1,
                      'COLOR_TOKEN' => 'primary',
                      'BLOCKS' => [
                          ['MESSAGE' => '[B]New Request[/B]'],
                          ['LINK' => ['NAME' => 'Open', 'LINK' => 'https://example.com']],
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
          MESSAGE: 'Card',
          ATTACH: {
              ID: 1,
              COLOR_TOKEN: 'primary',
              BLOCKS: [
                  { MESSAGE: '[B]New Request[/B]' },
                  { LINK: { NAME: 'Open', LINK: 'https://example.com' } },
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
          'MESSAGE' => 'Card',
          'ATTACH' => [
              'ID' => 1,
              'COLOR_TOKEN' => 'primary',
              'BLOCKS' => [
                  ['MESSAGE' => '[B]New Request[/B]'],
                  ['LINK' => ['NAME' => 'Open', 'LINK' => 'https://example.com']],
              ],
          ],
      ]
  );

  print_r($result);
  ```

{% endlist %}

{% note warning "" %}

The current documentation on attachments can be found in the Chatbots 2.0 section:

- [Attachments in Messages (Attach)](../../chat-bots/chat-bots-v2/imbot.v2/messages/attachments/index.md)

{% endnote %}

## Continue Learning

- [{#T}](../../chat-bots/chat-bots-v2/imbot.v2/messages/index.md)
- [{#T}](../../chat-bots/chat-bots-v2/imbot.v2/messages/chat-message-send.md)