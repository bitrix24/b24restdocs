# Rate Employee Performance in the imopenlines.session.head.vote Dialog

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: a manager with evaluation rights in the line settings

The method `imopenlines.session.head.vote` saves the manager's rating and comment for a completed dialog.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **SESSION_ID***
[`integer`](../../../data-types.md) | Session identifier. 

The identifier can be obtained using the [imopenlines.session.history.get](./imopenlines-session-history-get.md) method in the `sessionId` field ||
|| **RATING**
[`integer`](../../../data-types.md) | Manager's rating. Pass a value from `1` to `5` ||
|| **COMMENT**
[`string`](../../../data-types.md) | Manager's comment on the rating ||
|#

{% note info "" %}

At least one of the parameters must be provided: `RATING` or `COMMENT`.

{% endnote %} 

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"SESSION_ID":1743,"RATING":5,"COMMENT":"Excellent handling"}' \
      https://your-domain.bitrix24.com/rest/1/webhook_key/imopenlines.session.head.vote.json
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"SESSION_ID":1743,"RATING":5,"COMMENT":"Excellent handling","auth":"<access_token>"}' \
      https://your-domain.bitrix24.com/rest/imopenlines.session.head.vote.json
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
        method: 'imopenlines.session.head.vote',
        params: {
          SESSION_ID: 1743,
          RATING: 5,
          COMMENT: 'Excellent handling',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Vote saved:', result)
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
      async function submitHeadVote() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'imopenlines.session.head.vote',
            params: {
              SESSION_ID: 1743,
              RATING: 5,
              COMMENT: 'Excellent handling',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Vote saved:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', submitHeadVote)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'imopenlines.session.head.vote',
                [
                    'SESSION_ID' => 1743,
                    'RATING' => 5,
                    'COMMENT' => 'Excellent handling',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    } catch (Throwable $exception) {
        error_log($exception->getMessage());
        echo 'Error voting as head: ' . $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imopenlines.session.head.vote',
        {
            SESSION_ID: 1743,
            RATING: 5,
            COMMENT: 'Excellent handling',
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
        'imopenlines.session.head.vote',
        [
            'SESSION_ID' => 1743,
            'RATING' => 5,
            'COMMENT' => 'Excellent handling',
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Success: ' . print_r($result['result'], true);
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1773681878,
        "finish": 1773681878.850923,
        "duration": 0.8509230613708496,
        "processing": 0,
        "date_start": "2026-03-16T20:24:38+01:00",
        "date_finish": "2026-03-16T20:24:38+01:00",
        "operating_reset_at": 1773682478,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Returns `true` if the rating is saved ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "EMPTY_VOTE_PARAMS",
    "error_description": "At least one of the parameters RATING or COMMENT must be specified"
}
```

{% include notitle [Error Handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `EMPTY_SESSION_ID` | Session ID can't be empty | `SESSION_ID` not provided or value `<= 0` ||
|| `400` | `EMPTY_VOTE_PARAMS` | At least one of the parameters RATING or COMMENT must be specified | Both `RATING` and `COMMENT` not provided ||
|| `400` | `ACCESS_DENIED` | You cannot open this conversation as you do not have sufficient rights | The current user cannot rate the specified session ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imopenlines-session-open.md)
- [{#T}](./imopenlines-session-start.md)
- [{#T}](./imopenlines-session-join.md)
- [{#T}](./imopenlines-session-history-get.md)
- [{#T}](./imopenlines-session-intercept.md)
- [{#T}](./imopenlines-session-mode-pin.md)
- [{#T}](./imopenlines-session-mode-pin-all.md)
- [{#T}](./imopenlines-session-mode-unpin-all.md)
- [{#T}](./imopenlines-session-mode-silent.md)
- [{#T}](./imopenlines-message-session-start.md)
- [{#T}](./imopenlines-crm-lead-create.md)
- [{#T}](./imopenlines-dialog-get.md)