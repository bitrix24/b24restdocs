# Move emails to folder mail.message.movetofolder

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`mail`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the mailbox where the emails are located

The `mail.message.movetofolder` method moves emails to a folder, spam, or the trash.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **messageIds***
[`array`](../../../data-types.md) | An array of email IDs.

IDs can be obtained using the [mail.message.list](./mail-message-list.md) method.

In a single request, pass emails from only one folder and one mailbox only. ||
|| **action***
[`string`](../../../data-types.md) | Action for emails:

- `move` — move to the folder specified in `folder`
- `spam` — move to spam
- `delete` — move to trash ||
|| **folder**
[`string`](../../../data-types.md) | The name or path of an existing folder in this mailbox.

Required for the `move` action. ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` parameter to the request:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/mail.message.movetofolder`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"messageIds":[15,16],"action":"spam"}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/mail.message.movetofolder
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"messageIds":[15,16],"action":"spam","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/mail.message.movetofolder
    ```

- JS

    SDKs do not yet support the /rest/api/ address in calls. Use direct HTTP requests, for example, via curl or fetch.

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'mail.message.movetofolder',
            {
                messageIds: [15, 16],
                action: 'spam'
            }
        );

        const result = response.getData().result;
        console.log('Success:', result);
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    SDKs do not yet support the /rest/api/ address in calls. Use direct HTTP requests, for example, via curl or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'mail.message.movetofolder',
                [
                    'messageIds' => [15, 16],
                    'action' => 'spam'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the /rest/api/ address in calls. Use direct HTTP requests, for example, via curl or fetch.

    ```js
    BX24.callMethod(
        'mail.message.movetofolder',
        {
            messageIds: [15, 16],
            action: 'spam'
        },
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the /rest/api/ address in calls. Use direct HTTP requests, for example, via curl or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'mail.message.movetofolder',
        [
            'messageIds' => [15, 16],
            'action' => 'spam'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "success": true,
        "movedCount": 2,
        "action": "spam"
    },
    "time": {
        "start": 1779819678,
        "finish": 1779819678.84803,
        "duration": 0.8480300903320312,
        "processing": 0,
        "date_start": "2026-05-26T21:21:18+03:00",
        "date_finish": "2026-05-26T21:21:18+03:00",
        "operating_reset_at": 1779820278,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with the move result ||
|| **success**
[`boolean`](../../../data-types.md) | Sign of successful operation execution ||
|| **movedCount**
[`integer`](../../../data-types.md) | Number of processed emails ||
|| **action**
[`string`](../../../data-types.md) | Performed action ||
|| **time**
[`time`](../../../data-types.md#time) | Information about request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error during request object validation",
        "validation": [
            {
                "message": "Mandatory field is missing",
                "field": "messageIds"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors
Error code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to fix** ||
|| `messageIds` | Required field is not specified | Pass `messageIds` in the request body ||
|| `messageIds` | Select emails from a single folder | Pass emails from only one folder in a single request ||
|| `folder` | The `folder` parameter is required when `action=move` | Pass `folder` or use `action=spam` / `action=delete` ||
|| `action` | The `action` parameter must be `move`, `spam`, or `delete` | Pass one of the allowed values ||
|| `messageIds` | No accessible messages found | Pass existing email IDs that the user has permissions for ||
|| `messageIds` | All messages must belong to the same mailbox | Pass emails from only one mailbox in the request ||
|#

#### Access Errors
Error code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to fix** ||
|| `-` | Access denied | Check user permissions and `mail` scope ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./mail-message-list.md)
- [{#T}](./mail-message-get.md)