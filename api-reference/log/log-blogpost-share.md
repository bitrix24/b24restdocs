# Add Recipients to News Feed Message log.blogpost.share

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`log`](../scopes/permissions.md)
>
> Who can execute the method: administrator or message author

The method `log.blogpost.share` adds new recipients to a news feed message.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **POST_ID***
[`integer`](../data-types.md) | Identifier of the message.

You can obtain the identifier using the [log.blogpost.get](./log-blogpost-get.md) method ||
|| **DEST***
[`array`](../data-types.md) | List of new recipients who will be granted access to view the message. 

Possible values:

{% include notitle [message recipients](./_includes/log-recepients.md) %}
||
|| **USER_ID**
[`integer`](../data-types.md) | Identifier of the user on behalf of whom the message is edited. Available only to administrators.

The identifier can be obtained using the [user.get](../user/user-get.md) method.

By default, it is the current user who initiated the method call.
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"POST_ID":217,"DEST":["SG69","DR4"]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/log.blogpost.share
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"POST_ID":217,"DEST":["SG69","DR4"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/log.blogpost.share
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
        method: 'log.blogpost.share',
        params: {
          POST_ID: 217,
          DEST: ['SG69', 'DR4'],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Recipients added:', result)
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
      async function shareBlogPost() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'log.blogpost.share',
            params: {
              POST_ID: 217,
              DEST: ['SG69', 'DR4'],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Recipients added:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', shareBlogPost)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'log.blogpost.share',
                [
                    'POST_ID' => 217,
                    'DEST' => ['SG69', 'DR4']
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error sharing blog post: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'log.blogpost.share',
        {
            POST_ID: 217,
            DEST: ['SG69', 'DR4']
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
        'log.blogpost.share',
        [
            'POST_ID' => 217,
            'DEST' => ['SG69', 'DR4']
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
        "start": 1773814878,
        "finish": 1773814878.602921,
        "duration": 0.6029210090637207,
        "processing": 0,
        "date_start": "2026-03-18T09:21:18+02:00",
        "date_finish": "2026-03-18T09:21:18+02:00",
        "operating_reset_at": 1773815478,
        "operating": 0.284437894821167
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | `true` if recipients were added ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Wrong destinations"
}
```

{% include notitle [Error Handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| — | `Wrong post ID` | Invalid `POST_ID` ||
|| — | `Blog module not installed` | `blog` module is not installed ||
|| — | `Wrong destinations` | Invalid list of recipients `DEST` ||
|| — | `No read perms` | Insufficient permissions to read the message ||
|| — | No access to one or more message recipients | No permissions for one or more recipients from `DEST` ||
|#

{% include [System Errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./log-blogpost-add.md)
- [{#T}](./log-blogpost-update.md)
- [{#T}](./log-blogpost-get.md)
- [{#T}](./log-blogpost-delete.md)
- [{#T}](./log-blogpost-getusers-important.md)