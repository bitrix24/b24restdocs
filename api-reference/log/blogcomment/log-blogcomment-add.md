# Add a comment to the message log.blogcomment.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`log`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `log.blogcomment.add` adds a comment to a News Feed message.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **POST_ID***
[`integer`](../../data-types.md) | Identifier of the News Feed message.

The message identifier can be obtained using the method [log.blogpost.get](../log-blogpost-get.md) ||
|| **TEXT***
[`string`](../../data-types.md) | The text of the comment. It is considered during duplication checks and is saved in the `POST_TEXT` field ||
|| **FILES**
[`array`](../../data-types.md) | An array of files described according to the [file handling rules](../../files/how-to-upload-files.md). Files will be uploaded to the user's Drive and linked to the comment ||
|| **USER_ID**
[`integer`](../../data-types.md) | Identifier of the user on behalf of whom the comment is published. Available only to administrators. By default, the current user initiating the method call is used.

The user identifier can be obtained using the method [user.get](../../user/user-get.md) or [user.search](../../user/user-search.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"POST_ID":403,"TEXT":"Comment on the post","USER_ID":27,"FILES":[["example.txt","SXQncyBhIHRlc3QgZmlsZSBmb3IgQml0cml4IFJlc3QgQVBJLg=="]]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/log.blogcomment.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"POST_ID":403,"TEXT":"Comment on the post","USER_ID":27,"FILES":[["example.txt","SXQncyBhIHRlc3QgZmlsZSBmb3IgQml0cml4IFJlc3QgQVBJLg=="]],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/log.blogcomment.add
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
        method: 'log.blogcomment.add',
        params: {
          POST_ID: 403,
          TEXT: 'Comment on the post',
          USER_ID: 27,
          FILES: [
            [
              'example.txt',
              'SXQncyBhIHRlc3QgZmlsZSBmb3IgQml0cml4IFJlc3QgQVBJLg==',
            ],
          ],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Created comment ID:', result)
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
      async function addBlogComment() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'log.blogcomment.add',
            params: {
              POST_ID: 403,
              TEXT: 'Comment on the post',
              USER_ID: 27,
              FILES: [
                [
                  'example.txt',
                  'SXQncyBhIHRlc3QgZmlsZSBmb3IgQml0cml4IFJlc3QgQVBJLg==',
                ],
              ],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Created comment ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addBlogComment)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'log.blogcomment.add',
                [
                    'POST_ID' => 403,
                    'TEXT'    => 'Comment on the post',
                    'USER_ID' => 27,
                    'FILES'   => [
                        [
                            'example.txt',
                            'SXQncyBhIHRlc3QgZmlsZSBmb3IgQml0cml4IFJlc3QgQVBJLg==',
                        ],
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Created comment ID: ' . $result->data();
        }
    } catch (Throwable $exception) {
        error_log($exception->getMessage());
        echo 'Error adding blog comment: ' . $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod('log.blogcomment.add', {
        POST_ID: 403,
        TEXT: 'Comment on the post',
        USER_ID: 27,
        FILES: [
            [
                'example.txt',
                'SXQncyBhIHRlc3QgZmlsZSBmb3IgQml0cml4IFJlc3QgQVBJLg==',
            ],
        ]
    }, 
    function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'log.blogcomment.add',
        [
            'POST_ID' => 403,
            'TEXT'    => 'Comment on the post',
            'USER_ID' => 27,
            'FILES'   => [
                [
                    'example.txt',
                    'SXQncyBhIHRlc3QgZmlsZSBmb3IgQml0cml4IFJlc3QgQVBJLg==',
                ],
            ],
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Created comment ID: ' . $result['result'];
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": 312,
    "time": {
        "start": 1728904800.123456,
        "finish": 1728904800.398112,
        "duration": 0.2746560573577881,
        "processing": 0.10234594345092773,
        "date_start": "2025-10-14T12:40:00+02:00",
        "date_finish": "2025-10-14T12:40:00+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../data-types.md) | Identifier of the created comment ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "No blog module installed"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Errors

#|
|| **Code** | **Description** | **Value** ||
|| `-` | `No blog module installed` | The `blog` module is not installed ||
|| `-` | `No post found` | The News Feed message with the provided `POST_ID` was not found or is unavailable ||
|| `-` | `No blog found` | Failed to retrieve the blog associated with the message ||
|| `-` | `Duplicate comment` | A similar comment has already been published, checked only for posts without attachments ||
|| `-` | `No permissions` | The user does not have permission to add a comment to the message ||
|| `-` | `Blog comment hasn't been added` | Internal error while saving the comment ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./log-blogcomment-delete.md)
- [{#T}](./log-blogcomment-user-get.md)