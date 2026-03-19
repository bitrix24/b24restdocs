# Rate Employee Performance in the imopenlines.session.head.vote Dialog

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

- JS

    ```js
    try {
        const response = await $b24.callMethod(
            'imopenlines.session.head.vote',
            {
                SESSION_ID: 1743,
                RATING: 5,
                COMMENT: 'Excellent handling',
            }
        );

        const { result } = response.getData();
        console.log(result);
    } catch (error) {
        console.error(error);
    }
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