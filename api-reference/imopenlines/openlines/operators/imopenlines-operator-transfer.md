# Transfer a dialog to another operator or to another line imopenlines.operator.transfer

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with dialog permissions

The method `imopenlines.operator.transfer` transfers a dialog to another operator or to the queue of another open line.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CHAT_ID***
[`integer`](../../../data-types.md) | Identifier of the open line chat.

The identifier can be obtained using the [imopenlines.session.open](../sessions/imopenlines-session-open.md) or [imopenlines.dialog.get](../sessions/imopenlines-dialog-get.md) methods. ||
|| **TRANSFER_ID**
[`string`](../../../data-types.md)\|[`integer`](../../../data-types.md) | Universal parameter for transferring the dialog.

Allowed formats: `USER_ID` of the employee or the string `queue<QUEUE_ID>`.

The user identifier can be obtained using the [user.get](../../../user/user-get.md) and [user.search](../../../user/user-search.md) methods, and the queue identifier can be obtained using the [imopenlines.config.list.get](../imopenlines-config-list-get.md) method. ||
|| **USER_ID**
[`integer`](../../../data-types.md) | Identifier of the operator to whom the dialog should be transferred.

The user identifier can be obtained using the [user.get](../../../user/user-get.md) and [user.search](../../../user/user-search.md) methods. ||
|| **QUEUE_ID**
[`integer`](../../../data-types.md) | Identifier of the line to which the dialog should be transferred.

`QUEUE_ID` can be obtained using the [imopenlines.config.list.get](../imopenlines-config-list-get.md) method from the `ID` field. ||
|#

It is recommended to pass only one parameter: `TRANSFER_ID`, `USER_ID`, or `QUEUE_ID`.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"CHAT_ID":2043,"USER_ID":15}' \
      https://your-domain.bitrix24.com/rest/1/webhook_key/imopenlines.operator.transfer.json
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"CHAT_ID":2043,"USER_ID":15,"auth":"<access_token>"}' \
      https://your-domain.bitrix24.com/rest/imopenlines.operator.transfer.json
    ```

- JS

    ```js
    try {
        const response = await $b24.callMethod(
            'imopenlines.operator.transfer',
            {
                CHAT_ID: 2043,
                USER_ID: 15,
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
                'imopenlines.operator.transfer',
                [
                    'CHAT_ID' => 2043,
                    'USER_ID' => 15,
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
        echo 'Error transferring dialog: ' . $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imopenlines.operator.transfer',
        {
            CHAT_ID: 2043,
            USER_ID: 15,
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
        'imopenlines.operator.transfer',
        [
            'CHAT_ID' => 2043,
            'USER_ID' => 15,
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
        "date_start": "2026-03-16T15:10:32+02:00",
        "date_finish": "2026-03-16T15:10:32+02:00",
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
[`boolean`](../../../data-types.md) | Returns `true` if the dialog was successfully transferred. ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request. ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "TRANSFER_ID_EMPTY",
    "error_description": "Queue ID or User ID can't be empty"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `CHAT_ID_EMPTY` | Chat ID can't be empty | `CHAT_ID` not provided or value is `<= 0` ||
|| `400` | `USER_ID_EMPTY` | User ID can't be empty | Empty or invalid `USER_ID` provided ||
|| `400` | `QUEUE_ID_EMPTY` | QUEUE ID can't be empty | Empty or invalid `QUEUE_ID` provided ||
|| `400` | `TRANSFER_ID_EMPTY` | Queue ID or User ID can't be empty | `TRANSFER_ID`, `USER_ID`, and `QUEUE_ID` not provided ||
|| `400` | `OPERATOR_WRONG` | You cannot redirect to this operator | The target operator or line is unavailable for transferring the current dialog. ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imopenlines-operator-answer.md)
- [{#T}](./imopenlines-operator-finish.md)
- [{#T}](./imopenlines-operator-another-finish.md)
- [{#T}](./imopenlines-operator-skip.md)
- [{#T}](./imopenlines-operator-spam.md)