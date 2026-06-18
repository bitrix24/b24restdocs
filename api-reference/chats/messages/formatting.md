# Message Formatting

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

BB codes allow you to format the text of messages: highlight fragments, add links, line breaks, and special inserts.

Most often, formatting is conveyed in the `MESSAGE` field of the [im.message.add](./im-message-add.md) method.

## What You Can Do

- highlight text: `[B]`, `[I]`, `[U]`, `[S]`
- add a link: `[URL=...]...[/URL]`
- create a line break: `[BR]` or `\n`
- insert a command link: `[SEND=...]...[/SEND]`, `[PUT=...]...[/PUT]`

## Example of Sending a Formatted Message

{% include [Example Notes](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DIALOG_ID":"chat2725","MESSAGE":"[B]Important[/B][BR]Visit [URL=https://bitrix24.com]the website[/URL][BR][SEND=/help]Help[/SEND]"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.message.add
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DIALOG_ID":"chat2725","MESSAGE":"[B]Important[/B][BR]Visit [URL=https://bitrix24.com]the website[/URL][BR][SEND=/help]Help[/SEND]","auth":"**put_access_token_here**"}' \
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
          MESSAGE: '[B]Important[/B][BR]Open [URL=https://bitrix24.ru]site[/URL][BR][SEND=/help]Help[/SEND]',
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
      async function addMessage() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'im.message.add',
            params: {
              DIALOG_ID: 'chat2725',
              MESSAGE: '[B]Important[/B][BR]Open [URL=https://bitrix24.ru]site[/URL][BR][SEND=/help]Help[/SEND]',
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

      document.addEventListener('DOMContentLoaded', addMessage)
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
                  'MESSAGE' => '[B]Important[/B][BR]Visit [URL=https://bitrix24.com]the website[/URL][BR][SEND=/help]Help[/SEND]',
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
          MESSAGE: '[B]Important[/B][BR]Visit [URL=https://bitrix24.com]the website[/URL][BR][SEND=/help]Help[/SEND]',
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
          'MESSAGE' => '[B]Important[/B][BR]Visit [URL=https://bitrix24.com]the website[/URL][BR][SEND=/help]Help[/SEND]',
      ]
  );

  print_r($result);
  ```

{% endlist %}

{% note warning "" %}

The current documentation on formatting can be found in the Chat Bots 2.0 section:

- [Text Formatting (BB Codes)](../../chat-bots/chat-bots-v2/imbot.v2/messages/message-formatting.md)

{% endnote %}

## Continue Your Learning

- [{#T}](../../chat-bots/chat-bots-v2/imbot.v2/messages/index.md)
- [{#T}](../../chat-bots/chat-bots-v2/imbot.v2/messages/chat-message-send.md)