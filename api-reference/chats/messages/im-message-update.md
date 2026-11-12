# Update Message im.message.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: message author

The method `im.message.update` modifies the text and parameters of an already sent message.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **MESSAGE_ID***
[`integer`](../../data-types.md) | Identifier of the message.

The identifier is returned by the methods [im.message.add](./im-message-add.md) and [im.dialog.messages.get](./im-dialog-messages-get.md).

You can edit only your own message and only within three days after sending it. A forwarded message cannot be edited ||
|| **MESSAGE**
[`string`](../../data-types.md) | New text of the message. If an empty value is provided, the message will be deleted.

If the parameter is not provided, the message text will remain unchanged ||
|| **ATTACH**
[`object`](../../data-types.md)
[`string`](../../data-types.md) | Attachment with content blocks: images, links, files.

Pass an object with an array of blocks as the value of the `BLOCKS` key, for example, `{"BLOCKS":[{"MESSAGE":"Block text"}]}`, or the array of blocks itself. To remove the attachment, pass `N` or an empty value.

Read more in the [Attachments](./attachments.md) article ||
|| **KEYBOARD**
[`object`](../../data-types.md)
[`string`](../../data-types.md) | Buttons below the message that the user can interact with.

Pass an object with an array of buttons as the value of the `BUTTONS` key, for example, `{"BUTTONS":[{"TEXT":"Open","LINK":"https://example.com"}]}`, or the array of buttons itself. To remove the buttons, pass `N` or an empty value.

Read more in the [Working with Keyboards](./keyboards.md) article ||
|| **MENU**
[`object`](../../data-types.md)
[`string`](../../data-types.md) | Custom items in the message context menu, for example, a link or an option to copy text.

Pass an object with an array of items as the value of the `ITEMS` key, for example, `{"ITEMS":[{"TEXT":"Open","LINK":"https://example.com"}]}`, or the array of items itself. To remove the custom items, pass `N` or an empty value.

Read more in the [Context Menu](./menu.md) article ||
|| **URL_PREVIEW**
[`string`](../../data-types.md) | Whether to show a card with the page title, description, and image under a link in the message.

Allowed values:
- `Y` — show the card
- `N` — do not show ||
|| **IS_EDITED**
[`string`](../../data-types.md) | Whether to show the "edited" label on the message:
- `Y` — show
- `N` — do not show

Default is `Y`

{% note info "" %}

The parameter applies when `ATTACH` is changed. When `MESSAGE` is changed, the label may appear even if you pass `N`

{% endnote %} ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"MESSAGE_ID":34239,"MESSAGE":"Updated text"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.message.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"MESSAGE_ID":34239,"MESSAGE":"Updated text","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/im.message.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'im.message.update',
        params: {
          MESSAGE_ID: 34239,
          MESSAGE: 'Updated text',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Message updated:', result)
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
      async function updateMessage() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'im.message.update',
            params: {
              MESSAGE_ID: 34239,
              MESSAGE: 'Updated text',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Message updated:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateMessage)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.im.message.update(
            message_id=34239,
            message="Updated text",
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
                'im.message.update',
                [
                    'MESSAGE_ID' => 34239,
                    'MESSAGE' => 'Updated text'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating message: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.message.update',
        {
            MESSAGE_ID: 34239,
            MESSAGE: 'Updated text'
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
        'im.message.update',
        [
            'MESSAGE_ID' => 34239,
            'MESSAGE' => 'Updated text'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "im.message.update", b24.Params{
    	"MESSAGE_ID": 34239,
    	"MESSAGE":    "Updated text",
    })
    if err != nil {
    	return fmt.Errorf("im.message.update: %w", err)
    }

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("done:", ok)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1772623160,
        "finish": 1772623160.616113,
        "duration": 0.6161129474639893,
        "processing": 0,
        "date_start": "2026-03-04T14:19:20+01:00",
        "date_finish": "2026-03-04T14:19:20+01:00",
        "operating_reset_at": 1772623760,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | `true` if the request completed without an error. If only `MESSAGE_ID` is passed, the method also returns `true`, but the message does not change ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "MESSAGE_ID_ERROR",
    "error_description": "Message ID can't be empty"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `MESSAGE_ID_ERROR` | Message ID can't be empty | The `MESSAGE_ID` parameter is missing or its value is not greater than `0` ||
|| `403` | `CANT_EDIT_MESSAGE` | Time has expired for modification or you don't have access | The message was not found, has been deleted, was forwarded, was sent by another user, or was sent more than three days ago ||
|| `400` | `ATTACH_ERROR` | Incorrect attach params | Invalid `ATTACH` object ||
|| `400` | `ATTACH_OVERSIZE` | You have exceeded the maximum allowable size of attach | The size of `ATTACH` exceeds the allowable limit ||
|| `400` | `KEYBOARD_ERROR` | Incorrect keyboard params | Invalid `KEYBOARD` object ||
|| `400` | `KEYBOARD_OVERSIZE` | You have exceeded the maximum allowable size of keyboard | The size of `KEYBOARD` exceeds the allowable limit ||
|| `400` | `menu_ERROR` | Incorrect menu params | Invalid `MENU` object ||
|| `400` | `MENU_OVERSIZE` | You have exceeded the maximum allowable size of menu | The size of `MENU` exceeds the allowable limit ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-message-add.md)
- [{#T}](./im-message-delete.md)
- [{#T}](./im-message-like.md)
- [{#T}](./im-message-share.md)
- [{#T}](./im-message-command.md)
- [{#T}](./im-dialog-messages-get.md)
- [{#T}](./im-dialog-messages-search.md)
- [{#T}](./im-dialog-read.md)
- [{#T}](./im-dialog-unread.md)
- [{#T}](./im-dialog-writing.md)