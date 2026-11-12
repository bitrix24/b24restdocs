# Send Message im.message.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to send messages in chat

The method `im.message.add` sends a message to a chat.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **DIALOG_ID***
[`string`](../../data-types.md) | Identifier of the dialog in the format:

- `chatXXX` — group chat, where `XXX` is the chat identifier
- `sgXXX` — group or project chat, where `XXX` is the group identifier
- `XXX` — personal dialog, where `XXX` is the user identifier

The [im.chat.get](../im-chat-get.md) method returns the chat identifier. The [user.get](../../user/user-get.md) and [user.search](../../user/user-search.md) methods return the user identifier ||
|| **MESSAGE***
[`string`](../../data-types.md) | The text of the message. Required if `ATTACH` is not provided.

If the text is longer than 20,006 characters, the method keeps the first 20,000 without an error and appends ` (...)` at the end.

[Formatting](./formatting.md) is supported ||
|| **ATTACH**
[`object`](../../data-types.md)
[`string`](../../data-types.md) | Attachment with content blocks: images, links, files. The parameter is required if `MESSAGE` is not provided.

Read more in the [Attachments](./attachments.md) article ||
|| **KEYBOARD**
[`object`](../../data-types.md)
[`string`](../../data-types.md) | Buttons under the message that the user can interact with.

Read more in the [Working with Keyboards](./keyboards.md) article ||
|| **MENU**
[`object`](../../data-types.md)
[`string`](../../data-types.md) | Custom items in the message context menu, for example, a link or an option to copy text.

Read more in the [Context Menu](./menu.md) article ||
|| **SYSTEM**
[`string`](../../data-types.md) | Indicator of a system message.

Allowed values:
- `Y` — system message
- `N` — regular message

Default is `N` ||
|| **URL_PREVIEW**
[`string`](../../data-types.md) | Whether to show a card with the page title, description, and image under a link in the message.

Allowed values:
- `Y` — show the card
- `N` — do not show

Default is `Y` ||
|| **REPLY_ID**
[`integer`](../../data-types.md) | Identifier of the message you are replying to. The message must be in the same chat.

The identifier is returned by the [im.dialog.messages.get](./im-dialog-messages-get.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DIALOG_ID":"chat2941","MESSAGE":"Message text","SYSTEM":"N","URL_PREVIEW":"Y","REPLY_ID":34237}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.message.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DIALOG_ID":"chat2941","MESSAGE":"Message text","SYSTEM":"N","URL_PREVIEW":"Y","REPLY_ID":34237,"auth":"**put_access_token_here**"}' \
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
          DIALOG_ID: 'chat2941',
          MESSAGE: 'Message text',
          SYSTEM: 'N',
          URL_PREVIEW: 'Y',
          REPLY_ID: 34237,
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
      async function sendMessage() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'im.message.add',
            params: {
              DIALOG_ID: 'chat2941',
              MESSAGE: 'Message text',
              SYSTEM: 'N',
              URL_PREVIEW: 'Y',
              REPLY_ID: 34237,
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

      document.addEventListener('DOMContentLoaded', sendMessage)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.im.message.add(
            dialog_id="chat2941",
            message="Message text",
            system=False,
            url_preview=True,
            reply_id=34237,
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
                    'DIALOG_ID' => 'chat2941',
                    'MESSAGE' => 'Message text',
                    'SYSTEM' => 'N',
                    'URL_PREVIEW' => 'Y',
                    'REPLY_ID' => 34237
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
            DIALOG_ID: 'chat2941',
            MESSAGE: 'Message text',
            SYSTEM: 'N',
            URL_PREVIEW: 'Y',
            REPLY_ID: 34237
        },
        function(result)
        {
            if (result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'im.message.add',
        [
            'DIALOG_ID' => 'chat2941',
            'MESSAGE' => 'Message text',
            'SYSTEM' => 'N',
            'URL_PREVIEW' => 'Y',
            'REPLY_ID' => 34237
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
    	"DIALOG_ID":   "chat2941",
    	"MESSAGE":     "Message text",
    	"SYSTEM":      "N",
    	"URL_PREVIEW": "Y",
    	"REPLY_ID":    34237,
    })
    if err != nil {
    	return fmt.Errorf("im.message.add: %w", err)
    }

    var newID b24.ID
    if err := json.Unmarshal(res.Result, &newID); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("id:", newID)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": 34239,
    "time": {
        "start": 1772552939,
        "finish": 1772552939.091396,
        "duration": 0.09139609336853027,
        "processing": 0,
        "date_start": "2026-03-03T16:48:59+01:00",
        "date_finish": "2026-03-03T16:48:59+01:00",
        "operating_reset_at": 1772553539,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../data-types.md) | Identifier of the sent message ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "MESSAGE_EMPTY",
    "error_description": "Message can't be empty"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `MESSAGE_EMPTY` | Message can't be empty | The `MESSAGE` parameter is empty, and `ATTACH` is not passed ||
|| `400` | `MESSAGE_EMPTY` | Wrong message type | `MESSAGE` is not passed as a string ||
|| `400` | `USER_ID_EMPTY` | User ID can't be empty | The user identifier in `DIALOG_ID` is not greater than `0` ||
|| `400` | `USER_NOT_FOUND` | User not found | The user is not found ||
|| `400` | `CHAT_ID_EMPTY` | Chat ID can't be empty | The chat could not be determined by `DIALOG_ID`, for example, the `sgXXX` group has no chat ||
|| `400` | `CHAT_ID` | Error creating a message | A chat with this identifier is not found. The error text is returned in the language used in Bitrix24 ||
|| `400` | `CANCELED` | You cannot send messages to the specified chat | The current user cannot write to this chat. The error text is returned in the language used in Bitrix24 ||
|| `403` | `ACCESS_ERROR` | Action unavailable | Insufficient permissions to send the message ||
|| `403` | `ACCESS_ERROR` | It is forbidden to send messages to this chat | Sending messages is prohibited in the chat settings ||
|| `400` | `PARAMS_ERROR` | Incorrect params | The `DIALOG_ID` parameter is not passed ||
|| `400` | `PARAMS_ERROR` | Message isn't added | The message could not be created ||
|| `400` | `ATTACH_ERROR` | Incorrect attach params | Invalid `ATTACH` object ||
|| `400` | `ATTACH_OVERSIZE` | You have exceeded the maximum allowable size of attach | The size of `ATTACH` exceeds the allowable limit ||
|| `400` | `KEYBOARD_ERROR` | Incorrect keyboard params | Invalid `KEYBOARD` object ||
|| `400` | `MENU_ERROR` | Incorrect menu params | Invalid `MENU` object ||
|| `403` | `REPLY_ACCESS_ERROR` | Action unavailable | The `REPLY_ID` message is not found or there is no access to it ||
|| `403` | `REPLY_FROM_OTHER_CHAT_ERROR` | You can only reply to a message within the same chat | The `REPLY_ID` message is in another chat ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-message-update.md)
- [{#T}](./im-message-delete.md)
- [{#T}](./im-message-like.md)
- [{#T}](./im-message-share.md)
- [{#T}](./im-message-command.md)
- [{#T}](./im-dialog-messages-get.md)
- [{#T}](./im-dialog-messages-search.md)
- [{#T}](./im-dialog-read.md)
- [{#T}](./im-dialog-unread.md)
- [{#T}](./im-dialog-writing.md)