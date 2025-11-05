# Delete comment from log.blogcomment.delete

> Scope: [`log`](../../scopes/permissions.md)
>
> Who can execute the method: the comment author or a user with full write access

The method `log.blogcomment.delete` removes a comment from the News Feed.

## Method Parameters

{% include [Note about required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **COMMENT_ID***
[`integer`](../../data-types.md) | The identifier of the comment. The value must be greater than `0`.

The comment identifier can be obtained using the method [log.blogcomment.add](./log-blogcomment-add.md) ||
|| **USER_ID**
[`integer`](../../data-types.md) | The identifier of the user performing the deletion. Available only to administrators. By default, the current user who initiated the call is used.

The user identifier can be obtained using the method [user.get](../../user/user-get.md) or [user.search](../../user/user-search.md) ||
|#

## Code Examples

{% include [Note about examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"COMMENT_ID":197,"USER_ID":503}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/log.blogcomment.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"COMMENT_ID":197,"USER_ID":503,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/log.blogcomment.delete
    ```

- JS

    ```js
    try {
        const response = await $b24.callMethod(
            'log.blogcomment.delete',
            {
                COMMENT_ID: 197,
                USER_ID: 503,
            }
        );

        const result = response.getData().result;
        console.info(result);
    }
    catch( error )
    {
        console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'log.blogcomment.delete',
                [
                    'COMMENT_ID' => 197,
                    'USER_ID' => 503,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Deleted: ' . print_r($result->data(), true);
        }
    } catch (Throwable $exception) {
        error_log($exception->getMessage());
        echo 'Error deleting comment: ' . $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'log.blogcomment.delete',
        {
            COMMENT_ID: 197,
            USER_ID: 503,
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
        'log.blogcomment.delete',
        [
            'COMMENT_ID' => 197,
            'USER_ID'    => 503,
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Deleted: ' . print_r($result['result'], true);
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1728901200.123456,
        "finish": 1728901200.456789,
        "duration": 0.33333301544189453,
        "processing": 0.12000012397766113,
        "date_start": "2025-10-14T11:40:00+02:00",
        "date_finish": "2025-10-14T11:40:00+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | The root element of the response, contains `true` in case of success ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "Wrong comment ID"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Errors

#|
|| **Code** | **Description** | **Value** ||
|| `-` | `Wrong comment ID` | An empty or incorrect comment identifier was provided ||
|| `-`| `Blog module not installed` | The `blog` module is not installed ||
|| `-` | `No delete perms` | The user does not have permission to delete the comment ||
|| `-` | `No comment found` | A comment with the provided identifier was not found ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./log-blogcomment-add.md)
- [{#T}](./log-blogcomment-user-get.md)