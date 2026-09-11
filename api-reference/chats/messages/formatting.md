# Message Formatting

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

BB codes let you format message text: highlight fragments, add links and line breaks, insert icons, images, and dates.

## When to Use Formatting

Markup is needed when a message should read as formatted text rather than a single line: to highlight the main point, split it into lines, link to an employee or a chat, or show code without distortion.

For other tasks, the section has its own mechanisms:

- add buttons under the message — [keyboards](./keyboards.md)
- attach structured blocks, images, or tables — [attachments](./attachments.md)
- add items to the message context menu — [menu](./menu.md)

## Before You Start

- the [`im`](../../scopes/permissions.md) scope
- permission to send messages to the chat the message is addressed to
- a message no longer than 20,000 characters

Bitrix24 always retains the first 20,000 characters, and may truncate longer text at this boundary, appending ` (...)` at the end. The method does not return an error in this case, so check the length on your side.

The markup is passed in the `MESSAGE` field of the [im.message.add](./im-message-add.md) and [im.message.update](./im-message-update.md) methods. The codes are case-insensitive: `[b]` and `[B]` work the same way.

Bitrix24 retains the message together with the codes and parses them when the message is displayed in the messenger. When you retrieve the message with [im.dialog.messages.get](./im-dialog-messages-get.md), it returns the same text with the codes, not rendered markup.

## Supported Codes {#codes}

Below are the main codes for messages sent by the `im.message.*` methods. Chatbots support a wider set — see [Text Formatting (BB Codes)](../../chat-bots/chat-bots-v2/imbot.v2/messages/message-formatting.md) for the full reference. Processing details of individual codes are in [{#T}](#notes).

### Text Formatting

#|
|| **Code** | **What It Does** | **Example** ||
|| `[B]...[/B]` | Bold text | `[B]important[/B]` ||
|| `[I]...[/I]` | Italic | `[I]note[/I]` ||
|| `[U]...[/U]` | Underline | `[U]term[/U]` ||
|| `[S]...[/S]` | Strikethrough | `[S]canceled[/S]` ||
|| `[SIZE=N]...[/SIZE]` | Font size from 8 to 30 pixels. Bitrix24 raises smaller values to 8 and lowers larger ones to 30. The `px` and `pt` suffixes are allowed | `[SIZE=20]large[/SIZE]` ||
|| `[COLOR=#HEX]...[/COLOR]` | Text color, `#RGB` or `#RRGGBB` | `[COLOR=#ff0000]red[/COLOR]` ||
|| `[CODE]...[/CODE]` | Text with no code parsing inside | `[CODE]var x = [B];[/CODE]` ||
|#

### Line Breaks {#newline}

#|
|| **What to Pass** | **What It Does** | **Example** ||
|| `[BR]` | Line break | `first[BR]second` ||
|| The `\n` character | Line break | `first\nsecond` ||
|#

### Links and Mentions

#|
|| **Code** | **What It Does** | **Example** ||
|| `[URL]...[/URL]` | Link whose text matches the address | `[URL]https://example.com[/URL]` ||
|| `[URL=address]...[/URL]` | Link with custom text | `[URL=https://example.com]website[/URL]` ||
|| `[USER=id]...[/USER]` | Mention of an employee | `[USER=1]Klaus[/USER]` ||
|| `[USER=all]...[/USER]` | Mention of all chat participants | `[USER=all]Everyone[/USER]` ||
|| `[CHAT=id]...[/CHAT]` | Link to a chat | `[CHAT=5]Sales Department[/CHAT]` ||
|| `[CONTEXT=dialog/message]...[/CONTEXT]` | Link to a message in a dialog | `[CONTEXT=chat5/41017]context[/CONTEXT]` ||
|#

### Actions

#|
|| **Code** | **What It Does** | **Example** ||
|| `[SEND=text]...[/SEND]` | Link that sends text to the chat | `[SEND=/help]help[/SEND]` ||
|| `[SEND]text[/SEND]` | Same, but the link text is sent to the chat | `[SEND]/help[/SEND]` ||
|| `[PUT=text]...[/PUT]` | Link that inserts text into the input field | `[PUT=/help]insert[/PUT]` ||
|| `[PUT]text[/PUT]` | Same, but the link text is inserted into the input field | `[PUT]/help[/PUT]` ||
|| `[CALL=number]...[/CALL]` | Link that starts a call | `[CALL=+491700000000]call[/CALL]` ||
|| `[CALL]number[/CALL]` | Same, but the number is taken from the link text | `[CALL]+491700000000[/CALL]` ||
|#

### Inserts

#|
|| **Code** | **What It Does** | **Example** ||
|| `[ICON=address]` | Icon from an image URL. Also accepts `title`. The size is set with `size` or with `width` and `height`: if only one dimension is specified, the other becomes the same. Default is 20 pixels, maximum is 100 | `[ICON=https://example.com/i.png title=Done size=20]` ||
|| `[IMG SIZE=size]address[/IMG]` | Image. The size is `small`, `medium`, or `large`; with any other value, the code remains plain text | `[IMG SIZE=medium]https://example.com/p.png[/IMG]` ||
|| `[TIMESTAMP=timestamp FORMAT=format]` | Date and time from a Unix timestamp in the reader's time zone. The allowed formats are listed [below](#timestamp-formats) | `[TIMESTAMP=1789000000 FORMAT=SHORT_TIME_FORMAT]` ||
|#

#### Date and Time Formats {#timestamp-formats}

The `FORMAT` value must match one of the listed ones. If the format is not recognized, Bitrix24 displays the code itself as plain text.

#|
|| **Format** | **What It Shows** ||
|| `FORMAT_DATE` | Date ||
|| `FORMAT_DATETIME` | Date and time with seconds ||
|| `SHORT_DATE_FORMAT` | Numeric date ||
|| `MEDIUM_DATE_FORMAT` | Date with an abbreviated month name ||
|| `LONG_DATE_FORMAT` | Date with a full month name ||
|| `DAY_MONTH_FORMAT` | Day and full month name, without the year ||
|| `DAY_SHORT_MONTH_FORMAT` | Day and abbreviated month name, without the year ||
|| `SHORT_DAY_OF_WEEK_MONTH_FORMAT` | Abbreviated day of the week, day, and full month name ||
|| `SHORT_DAY_OF_WEEK_SHORT_MONTH_FORMAT` | Abbreviated day of the week, day, and abbreviated month name ||
|| `DAY_OF_WEEK_MONTH_FORMAT` | Full day of the week, day, and month name ||
|| `FULL_DATE_FORMAT` | Day of the week, date, and year ||
|| `SHORT_TIME_FORMAT` | Hours and minutes ||
|| `LONG_TIME_FORMAT` | Hours, minutes, and seconds ||
|#

What the result looks like depends on the language and settings of Bitrix24. The same `SHORT_TIME_FORMAT` code gives `00:26` in one case and `3:26 am` in another. The time is converted to the reader's time zone, so different chat participants see different values.

## Important Considerations {#notes}

Three cases where the behavior differs from what you might expect.

- **`[BR]` is not retained as a code.** On saving, Bitrix24 replaces `[BR]` and `[br]` with the `\n` character, so retrieving the message returns a line break rather than a tag. Mixed-case spelling remains in the text but is also displayed as a line break in the chat
- **`[IMG]` works only with a direct link to an image.** If the address points to a page or a file of another type, the code remains plain text in the message
- **`[DISK=id]` is not markup but an attachment marker.** Bitrix24 tries to attach the Drive file with this ID to the message and removes the code itself from the text

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
          MESSAGE: '[B]Important[/B][BR]Open [URL=https://bitrix24.com]site[/URL][BR][SEND=/help]Help[/SEND]',
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
              MESSAGE: '[B]Important[/B][BR]Open [URL=https://bitrix24.com]site[/URL][BR][SEND=/help]Help[/SEND]',
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

- Python

  ```python
  from b24pysdk.errors import BitrixAPIError, BitrixSDKException

  try:
      bitrix_response = client.im.message.add(
          dialog_id="chat2725",
          message="[B]Important[/B][BR]Open the [URL=https://bitrix24.com]website[/URL][BR][SEND=/help]Help[/SEND]",
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "im.message.add", b24.Params{
    	"DIALOG_ID": "chat2725",
    	"MESSAGE":   "[B]Important[/B][BR]Visit [URL=https://bitrix24.com]the website[/URL][BR][SEND=/help]Help[/SEND]",
    })
    if err != nil {
    	return fmt.Errorf("im.message.add: %w", err)
    }

    // The response comes as json.RawMessage — the method returns
    // the ID of the created message.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

{% note warning "" %}

The current documentation on formatting can be found in the Chat Bots 2.0 section:

- [Text Formatting (BB Codes)](../../chat-bots/chat-bots-v2/imbot.v2/messages/message-formatting.md)

{% endnote %}

## Continue Your Learning

- [{#T}](./im-message-add.md)
- [{#T}](./im-message-update.md)
- [{#T}](./keyboards.md)
- [{#T}](./attachments.md)
- [{#T}](./menu.md)
- [{#T}](./index.md)
- [{#T}](../../chat-bots/chat-bots-v2/imbot.v2/messages/index.md)
- [{#T}](../../chat-bots/chat-bots-v2/imbot.v2/messages/chat-message-send.md)