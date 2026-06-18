# Update Chat Avatar im.chat.updateAvatar

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: chat participant with permission to change the appearance

The method `im.chat.updateAvatar` updates the chat avatar.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CHAT_ID***
[`integer`](../../data-types.md) | Identifier of the chat.

The chat identifier can be obtained using the [im.chat.get](../im-chat-get.md) method ||
|| **AVATAR***
[`string`](../../data-types.md) | Image in [Base64](../../files/how-to-upload-files.md) format.

Maximum image size is 5000x5000 ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":2935,"AVATAR":"/9j/4QAYRXhpZgAAS...CgCgCgCgP/9k="}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.chat.updateAvatar
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":2935,"AVATAR":"/9j/4QAYRXhpZgAAS...CgCgCgCgP/9k=","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/im.chat.updateAvatar
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
        method: 'im.chat.updateAvatar',
        params: {
          CHAT_ID: 2935,
          AVATAR: '/9j/4QAYRXhpZgAAS...CgCgCgCgP/9k=',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Avatar updated:', result)
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
      async function updateChatAvatar() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'im.chat.updateAvatar',
            params: {
              CHAT_ID: 2935,
              AVATAR: '/9j/4QAYRXhpZgAAS...CgCgCgCgP/9k=',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Avatar updated:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateChatAvatar)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'im.chat.updateAvatar',
                [
                    'CHAT_ID' => 2935,
                    'AVATAR' => '/9j/4QAYRXhpZgAAS...CgCgCgCgP/9k='
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating chat avatar: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.chat.updateAvatar',
        {
            CHAT_ID: 2935,
            AVATAR: '/9j/4QAYRXhpZgAAS...CgCgCgCgP/9k=',
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
        'im.chat.updateAvatar',
        [
            'CHAT_ID' => 2935,
            'AVATAR' => '/9j/4QAYRXhpZgAAS...CgCgCgCgP/9k='
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
        "start": 1772459338,
        "finish": 1772459339.565761,
        "duration": 1.5657610893249512,
        "processing": 1,
        "date_start": "2026-03-02T13:48:58+01:00",
        "date_finish": "2026-03-02T13:48:59+01:00",
        "operating_reset_at": 1772459938,
        "operating": 0.7875189781188965
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | `true` if the chat avatar has been updated ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "AVATAR_ERROR",
    "error_description": "Avatar incorrect type"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `CHAT_ID_EMPTY` | Chat ID can't be empty | `CHAT_ID` not provided ||
|| `ACCESS_DENIED` | Access denied | Insufficient permissions to change the avatar ||
|| `ACCESS_ERROR` | Action unavailable | Operation not available for this chat ||
|| `ACCESS_ERROR` | The avatar of this chat cannot be changed | Cannot change the avatar of this chat ||
|| `AVATAR_ERROR` | Avatar incorrect type | The file is not of image type ||
|| `AVATAR_ERROR` | Avatar incorrect size (max 5000x5000) | Image size exceeds the limit ||
|| `WRONG_REQUEST` | Chat doesn't exist | The specified chat does not exist ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](../im-chat-add.md)
- [{#T}](../chat-users/im-chat-user-add.md)
- [{#T}](./im-chat-update-title.md)
- [{#T}](./im-chat-update-color.md)
- [{#T}](../im-chat-get.md)
- [{#T}](../im-dialog-get.md)
- [{#T}](../chat-users/im-chat-user-list.md)
- [{#T}](../chat-users/im-chat-user-delete.md)
- [{#T}](../chat-users/im-chat-leave.md)