# Add a comment to the message log.blogcomment.add

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

- JS

    ```js
    try {
      const response = await $b24.callMethod(
        'log.blogcomment.add',
        {
          POST_ID: 403,
          TEXT: 'Comment on the post',
          USER_ID: 27,
          FILES: [
            [
              'example.txt',
              'SXQncyBhIHRlc3QgZmlsZSBmb3IgQml0cml4IFJlc3QgQVBJLg==',
            ],
          ],
        }
      );

      const { result } = response.getData();
      console.log('Created comment:', result);
    } catch (error) {
      console.error('Error adding comment:', error);
    }
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