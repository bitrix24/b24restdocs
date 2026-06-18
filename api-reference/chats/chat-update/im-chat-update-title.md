# Update Chat Title im.chat.updateTitle

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: chat participant with permission to change the appearance

The method `im.chat.updateTitle` updates the chat title.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CHAT_ID***
[`integer`](../../data-types.md) | Chat identifier.

The chat identifier can be obtained using the [im.chat.get](../im-chat-get.md) method. ||
|| **TITLE***
[`string`](../../data-types.md) | Chat title.

The value is automatically trimmed of whitespace and line breaks. ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":2935,"TITLE":"Project Chat"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.chat.updateTitle
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":2935,"TITLE":"Project Chat","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/im.chat.updateTitle
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
        method: 'im.chat.updateTitle',
        params: {
          CHAT_ID: 2935,
          TITLE: 'Project Chat',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Chat title updated:', result)
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
      async function updateChatTitle() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'im.chat.updateTitle',
            params: {
              CHAT_ID: 2935,
              TITLE: 'Project Chat',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Chat title updated:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateChatTitle)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'im.chat.updateTitle',
                [
                    'CHAT_ID' => 2935,
                    'TITLE' => 'Project Chat'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating chat title: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.chat.updateTitle',
        {
            CHAT_ID: 2935,
            TITLE: 'Project Chat',
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
        'im.chat.updateTitle',
        [
            'CHAT_ID' => 2935,
            'TITLE' => 'Project Chat'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1772461866,
        "finish": 1772461866.894201,
        "duration": 0.8942010402679443,
        "processing": 0,
        "date_start": "2026-03-02T15:31:06+01:00",
        "date_finish": "2026-03-02T15:31:06+01:00",
        "operating_reset_at": 1772462466,
        "operating": 0.11278390884399414
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | `true` if the chat title was updated. ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time. ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "TITLE_EMPTY",
    "error_description": "Title can't be empty"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `CHAT_ID_EMPTY` | Chat ID can't be empty | `CHAT_ID` not provided. ||
|| `TITLE_EMPTY` | Title can't be empty | `TITLE` not provided or empty after trimming. ||
|| `ACCESS_ERROR` | Action unavailable | Operation not available for this chat. ||
|| `ACCESS_ERROR` | This chat cannot be renamed | This chat cannot be renamed. ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](../im-chat-add.md)
- [{#T}](../chat-users/im-chat-user-add.md)
- [{#T}](./im-chat-update-avatar.md)
- [{#T}](./im-chat-update-color.md)
- [{#T}](../im-chat-get.md)
- [{#T}](../im-dialog-get.md)
- [{#T}](../chat-users/im-chat-user-list.md)
- [{#T}](../chat-users/im-chat-user-delete.md)
- [{#T}](../chat-users/im-chat-leave.md)