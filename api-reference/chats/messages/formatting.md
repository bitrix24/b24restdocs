# Message Formatting

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

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

- JS

  ```js
  try {
      const response = await $b24.callMethod(
          'im.message.add',
          {
              DIALOG_ID: 'chat2725',
              MESSAGE: '[B]Important[/B][BR]Visit [URL=https://bitrix24.com]the website[/URL][BR][SEND=/help]Help[/SEND]',
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