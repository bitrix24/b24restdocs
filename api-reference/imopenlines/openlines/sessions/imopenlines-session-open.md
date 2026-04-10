# Get Chat by User Code imopenlines.session.open

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with access permission to the dialogue

The method `imopenlines.session.open` returns the chat ID of the open line based on the user code `USER_CODE`.

## Method Parameters

{% include [Footnote on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **USER_CODE***
[`string`](../../../data-types.md) | String code of the user for the external system channel. 

Code format: ```<connector>|<LINE_ID>|<CONNECTOR_CHAT_ID>|<CONNECTOR_USER_ID>```, where:
- `<connector>` — connector identifier: `livechat`, `telegram`, and others
- `<LINE_ID>` — identifier of the open line
- `<CONNECTOR_CHAT_ID>` — chat identifier in the channel
- `<CONNECTOR_USER_ID>` — user identifier in the channel

The value can be obtained using the method [imopenlines.dialog.get](./imopenlines-dialog-get.md) from the `entity_id` field or the method [imopenlines.session.history.get](./imopenlines-session-history-get.md) from `result.chat.<chatId>.entityId` ||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"USER_CODE":"livechat|22|1761|587"}' \
      https://your-domain.bitrix24.com/rest/1/webhook_key/imopenlines.session.open.json
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"USER_CODE":"livechat|22|1761|587","auth":"<access_token>"}' \
      https://your-domain.bitrix24.com/rest/imopenlines.session.open.json
    ```

- JS

    ```js
    try {
        const response = await $b24.callMethod(
            'imopenlines.session.open',
            {
                USER_CODE: 'livechat|22|1761|587',
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
                'imopenlines.session.open',
                [
                    'USER_CODE' => 'livechat|22|1761|587',
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
        echo 'Error opening chat: ' . $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imopenlines.session.open',
        {
            USER_CODE: 'livechat|22|1761|587',
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
        'imopenlines.session.open',
        [
            'USER_CODE' => 'livechat|22|1761|587',
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
    "result": {
        "chatId": "1763"
    },
    "time": {
        "start": 1773666416,
        "finish": 1773666416.279787,
        "duration": 0.2797870635986328,
        "processing": 0,
        "date_start": "2026-03-16T16:06:56+01:00",
        "date_finish": "2026-03-16T16:06:56+01:00",
        "operating_reset_at": 1773667016,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root object of the response [(detailed description)](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **chatId**
[`integer`](../../../data-types.md) | Identifier of the open line chat ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "You cannot open this conversation because you do not have sufficient permissions"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ACCESS_DENIED` | You cannot open this conversation because you do not have sufficient permissions | No access to the chat with the specified `USER_CODE` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imopenlines-session-start.md)
- [{#T}](./imopenlines-session-join.md)
- [{#T}](./imopenlines-session-history-get.md)
- [{#T}](./imopenlines-session-intercept.md)
- [{#T}](./imopenlines-session-mode-pin.md)
- [{#T}](./imopenlines-session-mode-pin-all.md)
- [{#T}](./imopenlines-session-mode-unpin-all.md)
- [{#T}](./imopenlines-session-mode-silent.md)
- [{#T}](./imopenlines-session-head-vote.md)
- [{#T}](./imopenlines-message-session-start.md)
- [{#T}](./imopenlines-crm-lead-create.md)
- [{#T}](./imopenlines-dialog-get.md)