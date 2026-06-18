# Attachments in Messages

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

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
          MESSAGE: 'Card',
          ATTACH: {
            ID: 1,
            COLOR_TOKEN: 'primary',
            BLOCKS: [
              { MESSAGE: '[B]New request[/B]' },
              { LINK: { NAME: 'Open', LINK: 'https://example.com' } },
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
      async function sendMessageWithAttach() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'im.message.add',
            params: {
              DIALOG_ID: 'chat2725',
              MESSAGE: 'Card',
              ATTACH: {
                ID: 1,
                COLOR_TOKEN: 'primary',
                BLOCKS: [
                  { MESSAGE: '[B]New request[/B]' },
                  { LINK: { NAME: 'Open', LINK: 'https://example.com' } },
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

      document.addEventListener('DOMContentLoaded', sendMessageWithAttach)
    </script>
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