# Attachments in Messages

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

`ATTACH` is a structured message attachment: a card built from blocks with text, links, images, files, tables, and dividers. The attachment is passed in the `ATTACH` parameter of the [im.message.add](./im-message-add.md) and [im.message.update](./im-message-update.md) methods.

The full description of the attachment fields and of all block types is in the reference [Attachments in Messages ATTACH](../../chat-bots/chat-bots-v2/imbot.v2/messages/attachments/index.md).

## When to Use ATTACH

- use ATTACH when you need a card built from several kinds of content: a heading, a link, an image, a file, a table of values. Formatted text in `MESSAGE` is not enough for this — the BB codes are described in the article [Message Formatting](./formatting.md)
- use it for read-only content that does not require click actions. Buttons under a message are added by `KEYBOARD` — [Working with Keyboards](./keyboards.md), items in the message menu are added by `MENU` — [Working with Context Menu](./menu.md)

The choice is not exclusive: `im.message.add` accepts `MESSAGE`, `ATTACH`, `KEYBOARD`, and `MENU` in a single call.

## What You Need Before You Start

- the [`im`](../../scopes/permissions.md) scope
- the permission to send messages to the chat the message is addressed to
- an attachment shorter than 60,000 characters in serialized form

Bitrix24 converts the whole attachment to JSON, together with the `ID`, `COLOR_TOKEN`, and `COLOR` fields, and compares the length of the resulting string with the limit. If the condition is not met, the message is not sent, and the method returns an error — the codes are collected in the "Error Handling" section of the [im.message.add](./im-message-add.md) page.

## How to Assemble an Attachment

1. Assemble an array of blocks. Each element is an object with a single top-level key, and this key defines the block type.
2. Wrap the array in an object with the `ID`, `COLOR_TOKEN`, and `COLOR` fields if the card metadata is required. Without metadata, pass the array as is.
3. Pass the result in the `ATTACH` parameter of the [im.message.add](./im-message-add.md) method. To replace the attachment in a sent message, call [im.message.update](./im-message-update.md) with a new `ATTACH` value.

`ID` sets the attachment number within the message, `COLOR_TOKEN` sets the color scheme of the card, and `COLOR` sets an explicit HEX color.

After a successful `im.message.add` call, the card appears in the chat, and the method returns the identifier of the created message. The attachment can be replaced with the `im.message.update` method as long as the message editing window has not expired.

### Block Types

- [`MESSAGE`](../../chat-bots/chat-bots-v2/imbot.v2/messages/attachments/block-collections/text.md) — a paragraph of text with BB codes
- [`LINK`](../../chat-bots/chat-bots-v2/imbot.v2/messages/attachments/block-collections/links.md) — a link with a caption
- [`USER`](../../chat-bots/chat-bots-v2/imbot.v2/messages/attachments/block-collections/user.md) — an employee card
- [`GRID`](../../chat-bots/chat-bots-v2/imbot.v2/messages/attachments/block-collections/grid.md) — "name — value" rows
- [`IMAGE`](../../chat-bots/chat-bots-v2/imbot.v2/messages/attachments/block-collections/images.md) — images
- [`FILE`](../../chat-bots/chat-bots-v2/imbot.v2/messages/attachments/block-collections/files.md) — a file with a download link
- [`DELIMITER`](../../chat-bots/chat-bots-v2/imbot.v2/messages/attachments/block-collections/delimiter.md) — a divider between the parts of a card

The parameters of each block are in the [ATTACH Block Collection](../../chat-bots/chat-bots-v2/imbot.v2/messages/attachments/block-collections/index.md).

## Two Forms of ATTACH

### Full Form

```js
ATTACH: {
    ID: 1,
    COLOR_TOKEN: 'primary',
    BLOCKS: [
        { MESSAGE: 'New request' },
        { LINK: { NAME: 'Open', LINK: 'https://example.com' } }
    ]
}
```

### Short Form

If the attachment metadata is not required, pass the array of blocks directly:

```js
ATTACH: [
    { MESSAGE: 'New request' },
    { LINK: { NAME: 'Open', LINK: 'https://example.com' } }
]
```

{% note warning "" %}

In the `im.message.*` methods, the attachment is passed in the `ATTACH` parameter at the top level of the request. In the `imbot.v2.*` chat bot methods, the same object is placed in `fields.attach`. An example from the reference will not work in `im.message.*` unless this wrapper is removed.

{% endnote %}

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

- Python

  ```python
  from b24pysdk.errors import BitrixAPIError, BitrixSDKException

  try:
      bitrix_response = client.im.message.add(
          dialog_id="chat2725",
          message="Card",
          attach={
              "ID": 1,
              "COLOR_TOKEN": "primary",
              "BLOCKS": [
                  {
                      "MESSAGE": "[B]New request[/B]",
                  },
                  {
                      "LINK": {
                          "NAME": "Open",
                          "LINK": "https://example.com",
                      },
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "im.message.add", b24.Params{
    	"DIALOG_ID": "chat2725",
    	"MESSAGE":   "Card",
    	"ATTACH": b24.Params{
    		"ID":          1,
    		"COLOR_TOKEN": "primary",
    		"BLOCKS": []b24.Params{
    			{
    				"MESSAGE": "[B]New Request[/B]",
    			},
    			{
    				"LINK": b24.Params{
    					"NAME": "Open",
    					"LINK": "https://example.com",
    				},
    			},
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("im.message.add: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape from the im.message.add method page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Continue Learning

- [{#T}](./im-message-add.md)
- [{#T}](./im-message-update.md)
- [{#T}](./formatting.md)
- [{#T}](./index.md)
- [{#T}](../../chat-bots/chat-bots-v2/imbot.v2/messages/attachments/index.md)
- [{#T}](../../chat-bots/chat-bots-v2/imbot.v2/messages/attachments/block-collections/index.md)