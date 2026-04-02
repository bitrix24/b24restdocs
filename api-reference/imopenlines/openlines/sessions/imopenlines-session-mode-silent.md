# Switch the dialog to silent mode imopenlines.session.mode.silent

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with access permission to the dialog

The method `imopenlines.session.mode.silent` enables or disables the silent messaging mode in the dialog.

{% note warning "DEPRECATED" %}

The development of this method has been halted, but it continues to function.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CHAT_ID***
[`integer`](../../../data-types.md) | Identifier of the open line chat.

The identifier can be obtained using the [imopenlines.session.open](./imopenlines-session-open.md) or [imopenlines.dialog.get](./imopenlines-dialog-get.md) methods ||
|| **ACTIVATE**
[`string`](../../../data-types.md) | Activation flag. `Y` — enable silent mode, any other value disables it. By default, the mode is off ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"CHAT_ID":2043,"ACTIVATE":"Y"}' \
      https://your-domain.bitrix24.com/rest/1/webhook_key/imopenlines.session.mode.silent.json
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"CHAT_ID":2043,"ACTIVATE":"Y","auth":"<access_token>"}' \
      https://your-domain.bitrix24.com/rest/imopenlines.session.mode.silent.json
    ```

- JS

    ```js
    try {
        const response = await $b24.callMethod(
            'imopenlines.session.mode.silent',
            {
                CHAT_ID: 2043,
                ACTIVATE: 'Y',
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
                'imopenlines.session.mode.silent',
                [
                    'CHAT_ID' => 2043,
                    'ACTIVATE' => 'Y',
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
        echo 'Error setting silent mode: ' . $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imopenlines.session.mode.silent',
        {
            CHAT_ID: 2043,
            ACTIVATE: 'Y',
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
        'imopenlines.session.mode.silent',
        [
            'CHAT_ID' => 2043,
            'ACTIVATE' => 'Y',
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
        "start": 1773678966,
        "finish": 1773678966.767325,
        "duration": 0.7673249244689941,
        "processing": 0,
        "date_start": "2026-03-16T19:36:06+01:00",
        "date_finish": "2026-03-16T19:36:06+01:00",
        "operating_reset_at": 1773679566,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Returns `true` if the mode has been applied ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "CHAT_ID",
    "error_description": "Chat ID is incorrect"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `CHAT_ID` | The provided chat identifier is incorrect | Empty or invalid `CHAT_ID` ||
|| `400` | `CHAT_TYPE` | The specified chat is not an open line | The specified chat does not belong to open lines ||
|| `400` | `ACCESS_DENIED` | You cannot open this conversation due to insufficient rights | The current user does not have access to the dialog ||
|| `400` | `USER_ID` | The provided user identifier is incorrect | The user on behalf of whom the method is executed is not defined ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imopenlines-session-open.md)
- [{#T}](./imopenlines-session-start.md)
- [{#T}](./imopenlines-session-join.md)
- [{#T}](./imopenlines-session-history-get.md)
- [{#T}](./imopenlines-session-intercept.md)
- [{#T}](./imopenlines-session-mode-pin.md)
- [{#T}](./imopenlines-session-mode-pin-all.md)
- [{#T}](./imopenlines-session-mode-unpin-all.md)
- [{#T}](./imopenlines-session-head-vote.md)
- [{#T}](./imopenlines-message-session-start.md)
- [{#T}](./imopenlines-crm-lead-create.md)
- [{#T}](./imopenlines-dialog-get.md)