# Create a Lead Based on the Dialogue imopenlines.crm.lead.create

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with access permission to the dialogue

The method `imopenlines.crm.lead.create` creates a CRM lead based on the current chat in the Open Channels.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CHAT_ID***
[`integer`](../../../data-types.md) | Identifier of the Open Channels chat. 

The identifier can be obtained using the [imopenlines.session.open](./imopenlines-session-open.md) or [imopenlines.dialog.get](./imopenlines-dialog-get.md) methods. ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"CHAT_ID":2043}' \
      https://your-domain.bitrix24.com/rest/1/webhook_key/imopenlines.crm.lead.create.json
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"CHAT_ID":2043,"auth":"<access_token>"}' \
      https://your-domain.bitrix24.com/rest/imopenlines.crm.lead.create.json
    ```

- JS

    ```js
    try {
        const response = await $b24.callMethod(
            'imopenlines.crm.lead.create',
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
                'imopenlines.crm.lead.create',
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
        echo 'Error creating lead: ' . $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imopenlines.crm.lead.create',
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
        'imopenlines.crm.lead.create',
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
        "start": 1773683282,
        "finish": 1773683283.095389,
        "duration": 1.0953888893127441,
        "processing": 1,
        "date_start": "2026-03-16T20:48:02+01:00",
        "date_finish": "2026-03-16T20:48:03+01:00",
        "operating_reset_at": 1773683882,
        "operating": 0.8203368186950684
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Returns `true` if the lead was successfully created ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "CHAT_TYPE",
    "error_description": "The specified chat is not an Open Channel"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `CHAT_ID` | The specified chat identifier is incorrect | Empty or invalid `CHAT_ID` ||
|| `400` | `CHAT_TYPE` | The specified chat is not an Open Channel | The specified chat does not belong to Open Channels ||
|| `400` | `ACCESS_DENIED` | You cannot open this conversation as you do not have sufficient rights | The current user does not have access to the dialogue ||
|| `400` | `USER_ID` | The specified user identifier is incorrect | User not defined for whom the method is executed ||
|| `400` | `ERROR_USER_NOT_OPERATOR` | Attempt to save a CRM entity by a user who is not an operator | The method was called by a user who is not the current operator of the Open Channel session ||
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
- [{#T}](./imopenlines-message-session-start.md)
- [{#T}](./imopenlines-dialog-get.md)