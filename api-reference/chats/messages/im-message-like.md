# Change Status to "Like" im.message.like

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: chat Participant

The method `im.message.like` sets or removes the "Like" mark for a message.

## Method Parameters

{% include [Footnote on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **MESSAGE_ID***
[`integer`](../../data-types.md) | Identifier of the message.

The identifier can be obtained using the method [im.dialog.messages.get](./im-dialog-messages-get.md) ||
|| **ACTION**
[`string`](../../data-types.md) | Action for reacting to the message.

Allowed values:
- `auto` — automatically toggle the current status: if there is no "Like" reaction, it will be set; if there is a reaction, it will be removed
- `plus` — set "Like"
- `minus` — remove "Like"
  
Default is `auto`  ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"MESSAGE_ID":34247,"ACTION":"plus"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.message.like
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"MESSAGE_ID":34247,"ACTION":"plus","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/im.message.like
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
        method: 'im.message.like',
        params: {
          MESSAGE_ID: 34247,
          ACTION: 'plus',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Like action result:', result)
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
      async function likeMessage() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'im.message.like',
            params: {
              MESSAGE_ID: 34247,
              ACTION: 'plus',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Like action result:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', likeMessage)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'im.message.like',
                [
                    'MESSAGE_ID' => 34247,
                    'ACTION' => 'plus'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error liking message: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.message.like',
        {
            MESSAGE_ID: 34247,
            ACTION: 'plus'
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
        'im.message.like',
        [
            'MESSAGE_ID' => 34247,
            'ACTION' => 'plus'
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
        "start": 1772625907,
        "finish": 1772625908.047015,
        "duration": 1.0470149517059326,
        "processing": 0,
        "date_start": "2026-03-04T15:05:07+01:00",
        "date_finish": "2026-03-04T15:05:08+01:00",
        "operating_reset_at": 1772626508,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | `true` if the action was executed successfully ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "WITHOUT_CHANGES",
    "error_description": "Action completed without changes"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `MESSAGE_ID_ERROR` | Message ID can't be empty | `MESSAGE_ID` is missing or invalid ||
|| `WITHOUT_CHANGES` | Action completed without changes | The reaction state was already the same ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-message-add.md)
- [{#T}](./im-message-update.md)
- [{#T}](./im-message-delete.md)