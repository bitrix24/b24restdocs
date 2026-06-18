# Set the "unread" flag for messages im.dialog.unread

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.dialog.unread` sets the "unread" flag for messages in a dialog starting from the specified message.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **DIALOG_ID***
[`string`](../../data-types.md) | Identifier of the chat in the format:

- `chatXXX` — chat
- `sgXXX` — group or project chat
- `XXX` — user identifier for personal chat

The chat identifier can be obtained using the [im.chat.get](../im-chat-get.md) method. The user identifier can be obtained using the [user.get](../../user/user-get.md) and [user.search](../../user/user-search.md) methods ||
|| **MESSAGE_ID***
[`integer`](../../data-types.md) | Identifier of the first unread message ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"DIALOG_ID":"chat1489","MESSAGE_ID":84869}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.dialog.unread
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"DIALOG_ID":"chat1489","MESSAGE_ID":84869,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.dialog.unread
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
        method: 'im.dialog.unread',
        params: {
          DIALOG_ID: 'chat1489',
          MESSAGE_ID: 84869,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Unread flag set:', result)
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
      async function setDialogUnread() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'im.dialog.unread',
            params: {
              DIALOG_ID: 'chat1489',
              MESSAGE_ID: 84869,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Unread flag set:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', setDialogUnread)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service->core->call(
            'im.dialog.unread',
            [
                'DIALOG_ID' => 'chat1489',
                'MESSAGE_ID' => 84869,
            ]
        );

        $result = $response->getResponseData()->getResult();
        print_r($result);
    } catch (Throwable $e) {
        error_log($e->getMessage());
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.dialog.unread',
        {
            DIALOG_ID: 'chat1489',
            MESSAGE_ID: 84869
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'im.dialog.unread',
        [
            'DIALOG_ID' => 'chat1489',
            'MESSAGE_ID' => 84869,
        ]
    );

    print_r($result);
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1772624862,
        "finish": 1772624862.090708,
        "duration": 0.09070801734924316,
        "processing": 0,
        "date_start": "2026-03-04T14:47:42+01:00",
        "date_finish": "2026-03-04T14:47:42+01:00",
        "operating_reset_at": 1772625462,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if the unread message flag was successfully set ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

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
|| `400` | `MESSAGE_ID_ERROR` | First unread message id can't be empty | The `MESSAGE_ID` parameter is not provided or is provided with a value less than or equal to `0` ||
|| `400` | `DIALOG_ID_EMPTY` | Dialog ID can't be empty | The `DIALOG_ID` parameter is not provided, is empty, or is in an incorrect format ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-dialog-messages-get.md)
- [{#T}](./im-dialog-messages-search.md)
- [{#T}](./im-dialog-read.md)
- [{#T}](./im-dialog-writing.md)