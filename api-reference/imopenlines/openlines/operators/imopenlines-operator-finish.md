# Finish Your Dialogue imopenlines.operator.finish

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with access permission to the dialogue

The method `imopenlines.operator.finish` ends the open line dialogue on behalf of the current operator.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CHAT_ID***
[`integer`](../../../data-types.md) | Identifier of the open line chat.

The identifier can be obtained using the [imopenlines.session.open](../sessions/imopenlines-session-open.md) or [imopenlines.dialog.get](../sessions/imopenlines-dialog-get.md) methods ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"CHAT_ID":2043}' \
      https://your-domain.bitrix24.com/rest/1/webhook_key/imopenlines.operator.finish.json
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"CHAT_ID":2043,"auth":"<access_token>"}' \
      https://your-domain.bitrix24.com/rest/imopenlines.operator.finish.json
    ```

- JS

    ```js
    try {
        const response = await $b24.callMethod(
            'imopenlines.operator.finish',
            {
                CHAT_ID: 2043,
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
                'imopenlines.operator.finish',
                [
                    'CHAT_ID' => 2043,
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
        echo 'Error finishing dialog: ' . $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imopenlines.operator.finish',
        {
            CHAT_ID: 2043,
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
        'imopenlines.operator.finish',
        [
            'CHAT_ID' => 2043,
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
        "start": 1773663032,
        "finish": 1773663032.493037,
        "duration": 0.49303698539733887,
        "processing": 0,
        "date_start": "2026-03-16T15:10:32+01:00",
        "date_finish": "2026-03-16T15:10:32+01:00",
        "operating_reset_at": 1773663632,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Returns `true` if the dialogue was successfully finished ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "CHAT_ID",
    "error_description": "The provided chat identifier is incorrect"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `CHAT_ID` | The provided chat identifier is incorrect | Empty or invalid `CHAT_ID` ||
|| `400` | `CHAT_TYPE` | The specified chat is not an open line | The specified chat does not belong to open lines ||
|| `400` | `ACCESS_DENIED` | You cannot open this conversation as you do not have sufficient rights | The current user does not have rights to the dialogue ||
|| `400` | `USER_ID` | The provided user identifier is incorrect | User not defined for whom the method is executed ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imopenlines-operator-answer.md)
- [{#T}](./imopenlines-operator-another-finish.md)
- [{#T}](./imopenlines-operator-skip.md)
- [{#T}](./imopenlines-operator-spam.md)
- [{#T}](./imopenlines-operator-transfer.md)