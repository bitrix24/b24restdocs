# Start a New Dialogue Based on the Message imopenlines.message.session.start

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with dialogue permissions

The method `imopenlines.message.session.start` initiates a new session and transfers the specified message into it.

## Method Parameters

{% include [Footnote on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CHAT_ID***
[`integer`](../../../data-types.md) | Identifier of the open line chat. 

The identifier can be obtained using the [imopenlines.session.open](./imopenlines-session-open.md) or [imopenlines.dialog.get](./imopenlines-dialog-get.md) methods. ||
|| **MESSAGE_ID***
[`integer`](../../../data-types.md) | Identifier of the message within the chat. 

The identifier can be obtained using the [imopenlines.session.history.get](./imopenlines-session-history-get.md) method in the keys of the `message` object. ||
|#

## Code Examples

{% include [Footnote on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"CHAT_ID":2043,"MESSAGE_ID":18971}' \
      https://your-domain.bitrix24.com/rest/1/webhook_key/imopenlines.message.session.start.json
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"CHAT_ID":2043,"MESSAGE_ID":18971,"auth":"<access_token>"}' \
      https://your-domain.bitrix24.com/rest/imopenlines.message.session.start.json
    ```

- JS

    ```js
    try {
        const response = await $b24.callMethod(
            'imopenlines.message.session.start',
            {
                CHAT_ID: 2043,
                MESSAGE_ID: 18971,
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
                'imopenlines.message.session.start',
                [
                    'CHAT_ID' => 2043,
                    'MESSAGE_ID' => 18971,
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
        echo 'Error starting session from message: ' . $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imopenlines.message.session.start',
        {
            CHAT_ID: 2043,
            MESSAGE_ID: 18971,
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
        'imopenlines.message.session.start',
        [
            'CHAT_ID' => 2043,
            'MESSAGE_ID' => 18971,
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
        "start": 1773682918,
        "finish": 1773682919.022549,
        "duration": 1.0225489139556885,
        "processing": 1,
        "date_start": "2026-03-16T20:41:58+01:00",
        "date_finish": "2026-03-16T20:41:59+01:00",
        "operating_reset_at": 1773683518,
        "operating": 0.14626193046569824
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Returns `true` if a new session has been started based on the message. ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time. ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "CHAT_ID",
    "error_description": "The provided chat identifier is invalid."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `CHAT_ID` | The provided chat identifier is invalid. | Empty or incorrect `CHAT_ID` ||
|| `400` | `CHAT_TYPE` | The specified chat is not an open line. | The specified chat does not belong to open lines. ||
|| `400` | `ACCESS_DENIED` | You cannot open this conversation as you do not have sufficient permissions. | The current user does not have access to the dialogue. ||
|| `400` | `USER_ID` | The provided user identifier is invalid. | The user on behalf of whom the method is executed is not defined. ||
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
- [{#T}](./imopenlines-session-mode-silent.md)
- [{#T}](./imopenlines-session-head-vote.md)
- [{#T}](./imopenlines-crm-lead-create.md)
- [{#T}](./imopenlines-dialog-get.md)