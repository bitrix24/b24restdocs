# Get User Comments log.blogcomment.user.get

> Scope: [`log`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `log.blogcomment.user.get` returns a list of comments on a News Feed post for the specified user and information about attached files.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **USER_ID**
[`integer`](../../data-types.md) | The identifier of the user whose comments need to be retrieved. If the identifier is not specified, the method will return comments from the current user.

If the request is made by an administrator on behalf of another user, the `text` field in the response may be empty.

The user identifier can be obtained using the [user.get](../../user/user-get.md) or [user.search](../../user/user-search.md) methods. ||
|| **FIRST_ID**
[`integer`](../../data-types.md) | The method will return comments with identifiers greater than the specified value. ||
|| **LAST_ID**
[`integer`](../../data-types.md) | The method will return comments with identifiers less than the specified value. ||
|| **LIMIT**
[`integer`](../../data-types.md) | The number of records in the response. The acceptable value is from `1` to `1000`. By default, `100` comments are returned. ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"USER_ID":28,"FIRST_ID":215,"LAST_ID":216}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/log.blogcomment.user.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"USER_ID":28,"FIRST_ID":215,"LAST_ID":216,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/log.blogcomment.user.get
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod(
        'log.blogcomment.user.get',
        {
          USER_ID: 28,
          FIRST_ID: 215,
          LAST_ID: 216,
        }
      );

      const { result } = response.getData();
      console.log('Comments:', result);
    } catch (error) {
      console.error('Error getting comments:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'log.blogcomment.user.get',
                [
                    'USER_ID' => 28,
                    'FIRST_ID' => 215,
                    'LAST_ID' => 216,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Comments: ' . print_r($result->data(), true);
        }
    } catch (Throwable $exception) {
        error_log($exception->getMessage());
        echo 'Error getting blog comments: ' . $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod('log.blogcomment.user.get', {
        USER_ID: 28,
        FIRST_ID: 215,
        LAST_ID: 216
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
        'log.blogcomment.user.get',
        [
            'USER_ID'  => 28,
            'FIRST_ID' => 215,
            'LAST_ID'  => 216,
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Comments: ' . print_r($result['result'], true);
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "comments": [
            {
                "id": 4821,
                "comment_id": 4821,
                "log_id": 13579,
                "date": "2025-01-28T11:05:42+02:00",
                "text": "I support the idea!",
                "attach": [
                    90210
                ]
            }
        ],
        "files": {
            "90210": {
                "id": 90210,
                "date": "2025-01-28T11:02:18+02:00",
                "type": "image",
                "name": "diagram.png",
                "size": 184320,
                "image": {
                    "width": 1280,
                    "height": 720
                },
                "authorId": 28,
                "authorName": "John Smith",
                "urlPreview": "https://example.bitrix24.com/disk/showPreview/90210",
                "urlShow": "https://example.bitrix24.com/disk/showFile/90210",
                "urlDownload": "https://example.bitrix24.com/disk/downloadFile/90210"
            }
        }
    },
    "time": {
        "start": 1728905100.112233,
        "finish": 1728905100.498321,
        "duration": 0.38608813285827637,
        "processing": 0.14598798751831055,
        "date_start": "2025-10-14T12:45:00+02:00",
        "date_finish": "2025-10-14T12:45:00+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root object of the response. Contains an array of comments and information about attached files. ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request. ||
|#

### Structure of the `result` Object

#|
|| **Name**
`type` | **Description** ||
|| **comments**
[`object[]`](../../data-types.md) | An array of comments. Each element contains the fields:
- `id`, `comment_id`, `log_id`, `date` — in ISO 8601 format
- `text` and `attach` — an array of file identifiers from the `FILES` array. ||
|| **files**
[`object`](../../data-types.md) | An associative array with file descriptions, where the key is the identifier of the Drive object. For each file, `id`, `date`, `type`, `name`, `size`, author information, and direct links `urlPreview`, `urlShow`, `urlDownload` are provided. ||
|#

## Error Handling

The method does not return errors.

If the user identifier is not specified or an incorrect value is provided, the method returns comments from the current user.

{% include [System errors](../../../_includes/system-errors.md) %}

- [{#T}](./log-blogcomment-add.md)
- [{#T}](./log-blogcomment-delete.md)