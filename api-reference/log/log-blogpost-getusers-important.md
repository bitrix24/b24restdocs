# View Users Who Read an Important Message log.blogpost.getusers.important

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`log`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `log.blogpost.getusers.important` returns an array of user IDs who have read an important message.

## Method Parameters

{% include [Note on Required Parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **POST_ID***
[`integer`](../data-types.md) | The identifier of the important message in the News Feed.

You can obtain the identifier using the method [log.blogpost.get](./log-blogpost-get.md) ||
|#

{% note info "" %}

The method returns no more than 500 user IDs who have read the important message.

{% endnote %}

## Code Examples

{% include [Note on Examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"POST_ID":221}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/log.blogpost.getusers.important
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"POST_ID":221,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/log.blogpost.getusers.important
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<string[]>({
        method: 'log.blogpost.getusers.important',
        params: {
          POST_ID: 221,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Users who read the important post:', result.length, result)
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
      async function getImportantPostUsers() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'log.blogpost.getusers.important',
            params: {
              POST_ID: 221,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Users who read the important post:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getImportantPostUsers)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'log.blogpost.getusers.important',
                [
                    'POST_ID' => 221
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error retrieving important users: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'log.blogpost.getusers.important',
        {
            POST_ID: 221
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error(), result.error_description());
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
        'log.blogpost.getusers.important',
        [
            'POST_ID' => 221
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
    "result": ["1269", "1271"],
    "time": {
        "start": 1773755868,
        "finish": 1773755868.215257,
        "duration": 0.215256929397583,
        "processing": 0,
        "date_start": "2026-03-17T16:57:48+01:00",
        "date_finish": "2026-03-17T16:57:48+01:00",
        "operating_reset_at": 1773756468,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../data-types.md) | An array of user IDs who have read the important message. A maximum of 500 entries.

An empty array indicates that the user does not have permission to view the message. ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time. ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Wrong post ID"
}
```

{% include notitle [Error Handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| — | `Wrong post ID` | Invalid `POST_ID` ||
|#

{% include [System Errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./log-blogpost-add.md)
- [{#T}](./log-blogpost-update.md)
- [{#T}](./log-blogpost-get.md)
- [{#T}](./log-blogpost-delete.md)
- [{#T}](./log-blogpost-share.md)